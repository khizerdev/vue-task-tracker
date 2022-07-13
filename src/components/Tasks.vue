<template>
    <div :key="task.id" v-for="task in tasks">
         <h3>
      {{ task.title }}
  
        </h3>
        <p>{{ task.day }}</p>
    </div>
</template>

<script>
export default {
    name: 'Tasks',
    props: {
        tasks: Array,
    }
}
</script>