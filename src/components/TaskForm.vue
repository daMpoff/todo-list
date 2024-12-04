<template>
    <div class="py-[1vh]">
        <input
            v-model="newTask"
            placeholder="Текст новой задачи"
            class="border p-2 mr-2"
        />
        <button @click="addTask" class="bg-blue-500 text-white p-2">
            Добавить задачу
        </button>
        <p v-if="error" class="text-red-500">{{ error }}</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newTask: "",
            error: "",
        };
    },
    methods: {
        addTask() {
            if (this.newTask.trim() === "") {
                this.error = "Задача не может быть пустой!";
                return;
            }
            this.error = "";
            const task = {
                id: Date.now(),
                text: this.newTask,
                completed: false,
            };
            this.$emit("add-task", task);
            this.newTask = "";
        },
    },
};
</script>
