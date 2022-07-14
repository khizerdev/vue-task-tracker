<template>
    <div :key="task.id" v-for="task in tasks">
      <Task @toggle-completed="$emit('toggle-completed' , task.id)" @delete-task="$emit('delete-task' , task.id)" :task="task"/>
    </div>
</template>

<script>

import Task from './Task.vue'

export default {
    name: 'Tasks',
    props: {
        tasks: Array,
    },
    emits: ["delete-task","toggle-completed"],
    components: {
        Task
    }
}
</script>