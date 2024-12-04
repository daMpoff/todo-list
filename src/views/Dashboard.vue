<template>
    <div class="p-4">
        <h1 class="text-2xl font-bold mb-4">Статистика по задачам</h1>
        <TaskStats :tasks="tasks" />
    </div>
</template>

<script>
import TaskStats from "../components/TaskStats.vue";

export default {
    components: {
        TaskStats,
    },
    data() {
        return {
            tasks: [],
        };
    },
    created() {
        // Загрузите задачи из локального хранилища или другого источника
        const savedTasks = localStorage.getItem("tasks");
        if (savedTasks) {
            this.tasks = JSON.parse(savedTasks);
        }
    },
};
</script>
