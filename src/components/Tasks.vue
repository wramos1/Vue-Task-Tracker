<template>
    <div :key="task.id" v-for="task in tasks">
        <Task @toggle-reminder="$emit('toggle-reminder', task.id)" @deleteTask="$emit('delete-task', task.id)"
            :task="task" />
    </div>
</template>

<script lang="ts">
import Task from './Task.vue';
import TaskObj from '../Task';
import { defineComponent, PropType } from 'vue';


export default defineComponent({
    name: "Tasks",
    props: {
        tasks: {
            required: true,
            type: [] as PropType<TaskObj[]>,
        },
        onDeleteTask: {
            type: Function,
            require: true
        },
    },
    components:
    {
        Task
    },
    emits: ['delete-task', 'toggle-reminder'],
})
</script>