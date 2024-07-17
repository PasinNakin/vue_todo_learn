<template>
    <img :src="logoURL" :alt="logoCaption" width="300" height="200" />

    <h1>{{ title }}</h1>

    <div>
        <span
            >You have {{ allTasks }} {{ allTasks > 1 ? "tasks" : "task" }} at
            the moment</span
        >
        <br />
        <input
            type="text"
            v-model="newTask"
            placeholder="Add a new task"
            @keyup.enter="addTask"
        />
        <button @click="addTask" :disabled="newTask.length < 1">
            Add Task
        </button>
    </div>

    <div v-if="newTask.length > 0">
        <h3>New Task preview</h3>
        <p>{{ newTask }}</p>
    </div>

    <ul>
        <li
            v-for="(task, index) in latest"
            :key="task.id"
            @click="finishTask(task)"
            :class="{ strikeout: task.finished }"
        >
            {{ index + 1 }}. {{ task.name }}

            <div v-if="task.finished">
                <button @click="removeTask(task.id)">Delete Task</button>
            </div>

            <div v-else-if="task.edit">
                <button>Edit Task</button>
            </div>

            <div v-else>
                <p>No Button</p>
            </div>
        </li>
    </ul>
</template>

<script>
export default {
    data() {
        return {
            title: "My To-Do App",
            newTask: "",
            logoURL:
                "https://images.unsplash.com/photo-1522199670076-2852f80289c3?q=80&w=2072&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
            logoCaption: "Learn how to build todo app with vue 3",
            tasks: [
                { id: 1, name: "Learn Vue js", finished: true },
                { id: 2, name: "Build a Vue app", finished: false },
                { id: 3, name: "Build Todo App", finished: false },
            ],
        };
    },
    methods: {
        addTask() {
            if (this.newTask.length < 1) return;
            this.tasks.push({
                id: this.tasks.length + 1,
                name: this.newTask,
                finished: false,
            });
            this.newTask = "";
        },
        finishTask(task) {
            task.finished = !task.finished;
        },

        removeTask(taskID) {
            this.tasks = this.tasks.filter((task) => {
                return task.id !== taskID;
            });
        },
    },
    computed: {
        allTasks() {
            return this.tasks.length;
        },
        latest() {
            return [...this.tasks].reverse();
        },
    },
};
</script>

<style>
.strikeout {
    text-decoration: line-through;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
