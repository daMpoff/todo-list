<template>
    <header
        class="bg-red-500 w-full fixed p-5 top-0 left-0 h-[5vh] flex items-center"
    >
        <nav class="w-[90%] mx-auto flex justify-start gap-3 text-2xl">
            <router-link to="/" class="text-white py-2 hover:underline"
                >Главная</router-link
            >
            <router-link to="/tasks" class="text-white py-2 hover:underline"
                >Задачи</router-link
            >
            <router-link to="/dashboard" class="text-white py-2 hover:underline"
                >Статистика</router-link
            >
        </nav>
    </header>
</template>

<script>
export default {
    name: "Header",
};
</script>

<style scoped>
.router-link-active {
    font-weight: bold;
    text-decoration: underline;
}
</style>
