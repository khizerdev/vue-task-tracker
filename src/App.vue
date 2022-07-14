<template>
  <div class="container">
    <Header @toggle-form="toggleForm" title="Task Tracker" :showAddForm="showAddForm"/>
      <div v-show="showAddForm">
        <AddTask @add-task="addTask"/>
      </div>
    <Tasks @toggle-completed="toggleCompleted" @delete-task="deleteTask" :tasks="tasks"/>
  </div>
</template>

<script>

import Header from './components/Header.vue'
import Tasks from './components/Tasks.vue'
import AddTask from './components/AddTask.vue'

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask
  },
  emits: ["delete-task","toggle-completed",'add-task'],
  data() {
    return {
      tasks : [],
      showAddForm: false
    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
  },
  methods: {
    async deleteTask(id) {
      if(confirm('Are you sure ?')) {

        const res = await fetch('api/tasks/'+id , {
        method: 'DELETE',
        headers: {
          'Content-type': 'application/json',
        }
      });
        res.status == 200 ? this.tasks = this.tasks.filter((task) => task.id !== id) : alert("Something went wrong");
        
      }
    },


    async toggleCompleted(id){

      const result = await this.fetchTask(id);
      const update = {...result , completed: !result.completed}

      const res = await fetch('api/tasks/'+id , {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(update)
      });

      const data = await res.json();
      console.log(data)

      this.tasks = this.tasks.map((task) => task.id === id ? {...task , completed:data.completed} : task)
    },


    async addTask(task) {

      const res = await fetch('api/tasks' , {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task)
      });

      const data = await res.json();

      this.tasks = [...this.tasks , data]
    },
    toggleForm() {
      this.showAddForm = !this.showAddForm
    },
    async fetchTasks(){
      let res = await fetch('api/tasks')
      const data = res.json()
   
      return data;
    },
    async fetchTask(id){
      let res = await fetch('api/tasks/'+id)
      const data = res.json()
   
      return data;
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
Footer
© 2022 GitHub, Inc.
