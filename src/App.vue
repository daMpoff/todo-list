<template>
    <div class="min-h-screen bg-slate-300 flex flex-col items-center">
        <div class="w-[90%] mx-auto">
            <Header></Header>
            <main class="pt-[5vh]">
                <router-view />
            </main>
        </div>
    </div>
</template>

<script>
import Header from "./components/Header.vue";

export default {
    name: "App",
    components: {
        Header,
    },
};
</script>

<style scoped>
.router-link-active {
    font-weight: bold;
}
</style>
