<template>
    <div>
        <div v-for="task in tasks" :key="task.id" class="mb-4">
            <div class="flex mb-2 gap-3 items-center">
                <input
                    type="checkbox"
                    v-model="task.completed"
                    @change="syncSubTasks(task)"
                    class="mr-2"
                />
                <span
                    :class="{ 'line-through': task.completed }"
                    class="flex-grow"
                >
                    {{ task.text }}
                </span>
                <div class="flex gap-3 ml-auto">
                    <button @click="editTask(task.id)" class="text-blue-500">
                        Редактировать
                    </button>
                    <button @click="deleteTask(task.id)" class="text-red-500">
                        Удалить
                    </button>
                    <button
                        @click="openSubTaskForm(task.id)"
                        class="text-green-500"
                    >
                        Добавить подзадачу
                    </button>
                </div>
            </div>

            <!-- Отображение подзадач -->
            <div
                v-if="task.subTasks && task.subTasks.length > 0"
                class="ml-4 mb-2"
            >
                <div
                    v-for="subTask in task.subTasks"
                    :key="subTask.id"
                    class="flex items-center mb-1"
                >
                    <input
                        type="checkbox"
                        v-model="subTask.completed"
                        class="mr-2"
                    />
                    <span :class="{ 'line-through': subTask.completed }">{{
                        subTask.text
                    }}</span>
                </div>
            </div>
        </div>

        <!-- Модальное окно для редактирования задачи -->
        <div
            v-if="editingTask"
            class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-50"
        >
            <div class="bg-white p-4 rounded shadow-lg">
                <h2 class="text-2xl font-bold mb-2">Редактировать задачу</h2>
                <input
                    v-model="editingTask.text"
                    @keyup.enter="saveEdit"
                    class="border p-2 w-full mb-2"
                />
                <p v-if="error" class="text-red-500 mb-2">{{ error }}</p>
                <div class="flex justify-end">
                    <button @click="cancelEdit" class="text-gray-500 mr-2">
                        Отмена
                    </button>
                    <button @click="saveEdit" class="text-blue-500">
                        Сохранить
                    </button>
                </div>
            </div>
        </div>

        <!-- Модальное окно для добавления подзадачи -->
        <div
            v-if="addingSubTask"
            class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-50"
        >
            <div class="bg-white p-4 rounded shadow-lg">
                <h2 class="text-2xl font-bold mb-2">Добавить подзадачу</h2>
                <SubTaskForm @add-subtask="addSubTask($event)" />
                <div class="flex justify-end">
                    <button
                        @click="cancelAddSubTask"
                        class="text-gray-500 mr-2"
                    >
                        Отмена
                    </button>
                    <button @click="saveSubTask" class="text-blue-500">
                        Сохранить
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import SubTaskForm from "./SubTaskForm.vue";

export default {
    components: {
        SubTaskForm,
    },
    props: {
        tasks: Array,
    },
    data() {
        return {
            editingTask: null,
            addingSubTask: null,
            error: "",
        };
    },
    methods: {
        deleteTask(id) {
            this.$emit("delete-task", id);
        },
        editTask(id) {
            const task = this.tasks.find((task) => task.id === id);
            if (task) {
                this.editingTask = { ...task, editing: true };
            }
        },
        saveEdit() {
            if (this.editingTask.text.trim() === "") {
                this.error = "Текст задачи не может быть пустым";
                return;
            }
            this.error = "";
            if (this.editingTask) {
                this.$emit("edit-task", this.editingTask.id, this.editingTask);
                this.editingTask = null;
            }
        },
        cancelEdit() {
            this.editingTask = null;
            this.error = "";
        },
        openSubTaskForm(taskId) {
            this.addingSubTask = taskId;
        },
        addSubTask(subTask) {
            const task = this.tasks.find(
                (task) => task.id === this.addingSubTask
            );
            if (task) {
                if (!task.subTasks) {
                    task.subTasks = [];
                }
                task.subTasks.push(subTask);
                this.$emit("edit-task", task.id, task);
                this.cancelAddSubTask();
            }
        },
        cancelAddSubTask() {
            this.addingSubTask = null;
        },
        syncSubTasks(task) {
            if (task.subTasks) {
                task.subTasks.forEach((subTask) => {
                    subTask.completed = task.completed;
                });
            }
        },
    },
};
</script>
