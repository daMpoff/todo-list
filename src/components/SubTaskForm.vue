<template>
    <div class="p-4">
        <input
            v-model="newSubTask"
            placeholder="Текст подзадачи"
            class="border p-2 mr-2"
        />
        <button @click="addSubTask" class="bg-blue-500 text-white p-2">
            Добавить подзадачу
        </button>
        <p v-if="error" class="text-red-500">{{ error }}</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newSubTask: "",
            error: "",
        };
    },
    methods: {
        addSubTask() {
            if (this.newSubTask.trim() === "") {
                this.error = "";
                return;
            }
            this.error = "";
            const subTask = {
                id: Date.now(),
                text: this.newSubTask,
                completed: false,
            };
            this.$emit("add-subtask", subTask);
            this.newSubTask = "";
        },
    },
};
</script>
