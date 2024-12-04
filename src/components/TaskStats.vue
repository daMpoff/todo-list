<template>
    <div>
        <p>Общее количество задач: {{ totalTasks }}</p>
        <p>Завершенные задачи: {{ completedTasks }}</p>
        <p>Активные задачи: {{ activeTasks }}</p>
    </div>
</template>

<script>
export default {
    props: {
        tasks: Array,
    },
    computed: {
        totalTasks() {
            return this.tasks.length;
        },
        completedTasks() {
            return this.tasks.filter((task) => task.completed).length;
        },
        activeTasks() {
            return this.tasks.filter((task) => !task.completed).length;
        },
    },
};
</script>
