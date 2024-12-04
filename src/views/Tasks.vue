<template>
    <div class="w-full p-5">
        <h1 class="text-4xl font-bold">Задачи</h1>
        <TaskForm @add-task="addTask" />
        <div class="flex mb-4 gap-5">
            <button @click="toggleSort('alphabet')" class="text-black text-lg">
                Сортировать по алфавиту ({{
                    sortDirections.alphabet === "asc" ? "↑" : "↓"
                }})
            </button>
            <button @click="toggleSort('date')" class="text-black text-lg">
                Сортировать по дате ({{
                    sortDirections.date === "asc" ? "↑" : "↓"
                }})
            </button>
        </div>
        <TaskList
            :tasks="sortedTasks"
            @delete-task="deleteTask"
            @edit-task="editTask"
        />
    </div>
</template>

<script>
import TaskForm from "../components/TaskForm.vue";
import TaskList from "../components/TaskList.vue";

export default {
    components: {
        TaskForm,
        TaskList,
    },
    data() {
        return {
            tasks: [],
            sortOrder: "date", // По умолчанию сортировка по дате
            sortDirections: {
                alphabet: "asc", // По умолчанию сортировка по возрастанию
                date: "asc", // По умолчанию сортировка по возрастанию
            },
        };
    },
    created() {
        this.loadTasks();
    },
    computed: {
        sortedTasks() {
            let sorted = [...this.tasks];
            if (this.sortOrder === "alphabet") {
                sorted.sort((a, b) => {
                    const comparison = a.text.localeCompare(b.text);
                    return this.sortDirections.alphabet === "asc"
                        ? comparison
                        : -comparison;
                });
            } else {
                sorted.sort((a, b) => {
                    const comparison = a.id - b.id;
                    return this.sortDirections.date === "asc"
                        ? comparison
                        : -comparison;
                });
            }
            return sorted;
        },
    },
    methods: {
        addTask(task) {
            this.tasks.push(task);
            this.saveTasks();
        },
        deleteTask(id) {
            this.tasks = this.tasks.filter((task) => task.id !== id);
            this.saveTasks();
        },
        editTask(id, updatedTask) {
            const index = this.tasks.findIndex((task) => task.id === id);
            if (index !== -1) {
                this.tasks[index] = updatedTask;
                this.saveTasks();
            }
        },
        saveTasks() {
            localStorage.setItem("tasks", JSON.stringify(this.tasks));
        },
        loadTasks() {
            const savedTasks = localStorage.getItem("tasks");
            if (savedTasks) {
                this.tasks = JSON.parse(savedTasks);
            }
        },
        toggleSort(order) {
            if (this.sortOrder === order) {
                this.sortDirections[order] =
                    this.sortDirections[order] === "asc" ? "desc" : "asc";
            } else {
                this.sortOrder = order;
                this.sortDirections[order] = "asc";
            }
        },
    },
};
</script>
