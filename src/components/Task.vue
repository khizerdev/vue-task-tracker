<template>
    <div @dblclick="$emit('toggle-completed' , task.id)" :class="[task.completed ? 'completed' : '' , 'task']">
        <h3>
        {{ task.title }}
        <i @click="onDelete(task.id)" class="fas fa-times"></i>
        </h3>
        <p>{{ task.date }}</p>
    </div>
</template>

<script>
export default {
    name: 'Task',
    props : {
        task: Object
    },
    methods: {
        onDelete(id) {
            this.$emit('delete-task' , id)
        }
    }

}
</script>

<style scope>
.fas {
  color: red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.completed {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>