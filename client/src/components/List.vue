<template>
  <div class="container" id="todo-list">
    <div class="row">
      <div class="col-md-6 mx-auto">
        <h1 class="text-center">TODO List App</h1>
        <form v-on:submit.prevent="addNewTask">
          <label for="tasknameinput">Task Name</label>
          <input v-model="taskname" class="form-control" id="tasknameinput" placeholder="Task Name">
          <button v-if="this.isEdit == false" type="submit" class="btn btn-success btn-block mt-3">
            Submit
          </button>
          <button v-else type="button" v-on:click="updateTask()" class="btn btn-primary btn-block mt-3">
            Update
          </button>
        </form>
        <table class="table">
          <tr v-for="(task) in tasks" v-bind:key="task.id" v-bind:task_name="task.task_name">
            <td class="text-left">{{task.task_name}}</td>
            <td class="text-right">
              <button v-on:click="editTask(task.task_name, task.id)" class="btn btn-info">Edit</button>
              <button v-on:click="deleteTask(task.id)" class="btn btn-danger">Delete</button>
            </td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      tasks: [],
      id: '',
      taskname: '',
      isEdit: false
    }
  },

  mounted() {
    this.getTasks()
  },

  methods: {
    getTasks() {
      axios.get('api/task')
      .then(
        result => {
          console.log(result.data)
          this.tasks = result.data
        },
        error => {
          console.error(error)
        }
      )
    },

    addNewTask() {
      axios.post('api/task', 
      {task_name: this.taskname})
      .then((res) => {
        this.taskname = ''
        this.getTasks()
        console.log(res)
      })
      .catch((err) => {
        console.log(err)
      })
    },

    editTask(title, id) {
      this.id = id
      this.taskname = title
      this.isEdit = true
    },

    updateTask() {
      axios.put(`api/task/${this.id}`, 
      {task_name: this.taskname})
      .then((res) => {
        this.taskname = ''
        this.isEdit = false
        this.getTasks()
        console.log(res)
      })
      .catch((err) => {
        console.log(err)
      })
    },

    deleteTask(id) {
      axios.delete(`api/task/${id}`)
      .then((res) => {
        this.taskname = ''
        this.getTasks()
        console.log(res)
      })
      .catch((err) => {
        console.log(err)
      })
    }
  }
}

</script>