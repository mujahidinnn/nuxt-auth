<script lang="ts" setup>
const emit = defineEmits(["success"]);

const { login } = useAuth();

const form = reactive({
    data: {
        email: "admin@gmail.com",
        password: "password",
        rememberMe: false,
    },
    error: "",
    pending: false,
    visible: false,
});

async function onLoginClick() {
    try {
        form.error = "";
        form.pending = true;

        await login(form.data.email, form.data.password, form.data.rememberMe);

        emit("success");
    } catch (error: any) {
        console.error(error);

        if (error.data.message) form.error = error.data.message;
    } finally {
        form.pending = false;
    }
}

function handleVisibility() {
    return (form.visible = !form.visible);
}
</script>

<template>
    <p v-if="form.error" class="mb-3 text-red-500">
        {{ form.error }}
    </p>
    <form class="mb-3 flex flex-col gap-3" @submit.prevent="onLoginClick">
        <div class="w-full">
            <input id="remember-me" v-model="form.data.rememberMe" type="checkbox" />

            <label for="remember-me" class="ml-1 text-light-100">Remember me</label>
        </div>
        <input v-model="form.data.email" type="email" placeholder="Email" required />
        <input
            v-model="form.data.password"
            :type="form.visible ? 'password' : 'text'"
            placeholder="Password"
            required
        />
        <a class="text-light-100 cursor-pointer text-sm underline" @click="handleVisibility">{{
            form.visible ? "Show" : "Hide"
        }}</a>
        <button
            type="submit"
            :disabled="form.pending"
            class="py-1 px-2 rounded bg-light-100 hover:bg-light-700 transition-colors"
        >
            Login
        </button>
    </form>
</template>
