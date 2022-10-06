<template>
    <AddTask @addTask="addTask" v-show="showAddTask" />
    <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="toggleReminder" />
</template>

<script lang="ts">
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';
import TaskObj from '../Task'
import { defineComponent } from 'vue';

export default defineComponent({
    name: 'Home',
    props: {
        showAddTask: Boolean
    },
    components: {
        Tasks,
        AddTask
    },
    data() {
        return {
            tasks: [] as TaskObj[]
        }
    },
    methods: {
        async deleteTask(id: number) {
            if (confirm('Are you sure you want to delete this task?')) {
                const response = await fetch(`api/tasks/${id}`, {
                    method: 'DELETE',
                })

                response.status === 200 ? (this.tasks = this.tasks.filter((task: TaskObj) => task.id !== id)) : alert('Error deleting task')
            }
        },
        async toggleReminder(id: number) {
            const taskToToggle = await this.fetchTask(id);
            const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

            const response = await fetch(`api/tasks/${id}`, {
                method: 'PUT',
                headers: {
                    'Content-type': 'application/json',
                },
                body: JSON.stringify(updTask)
            })

            const data = await response.json();

            this.tasks = this.tasks.map((task: TaskObj) =>
                task.id === id ? { ...task, reminder: data.reminder } : task
            )
        },
        async addTask(task: TaskObj) {
            const response = await fetch('api/tasks', {
                method: 'POST',
                headers: {
                    'Content-type': 'application/json',
                },
                body: JSON.stringify(task)
            })

            const data = await response.json();

            this.tasks = [...this.tasks, data]
        },
        async fetchTasks() {
            const res = await fetch('api/tasks');

            const data = await res.json();

            return data;
        },
        async fetchTask(id: number) {
            const res = await fetch(`api/tasks/${id}`);

            const data = await res.json();

            return data;
        },
        async created() {
            this.tasks = await this.fetchTasks();
        }
    }
})
</script>