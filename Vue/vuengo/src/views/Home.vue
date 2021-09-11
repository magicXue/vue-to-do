<template>
  <div class="home">
    <h1 class="title">Vuengo</h1>
  
    <hr>
    <div class="columns">
      <div class="column is-3 is-offset-3">
        <form>
          <h2 class="subtitle">Add task</h2>
          <div class="field">
            <label for="" class="label">Description</label>
            <div class="control">
              <input type="text" class="input">
            </div>
          </div>

          <div class="field">
            <label class="label">Status</label>
            <div class="control">
              <div class="select">
                <select>
                  <option value="todo">To do</option>
                  <option value="done">Done</option>
                </select>
              </div>
            </div>
          </div>

          <div class="fielld">
            <div class="control">
              <button class="button is-link">Submit</button>
            </div>
          </div>
        </form>
      </div>
    </div>
    <div class="columns">
      <div class="column is-6">
        <h2 class="subtitle">To do</h2>
        <div class="todo">
          <div class="card" v-for="task in tasks" v-if="task.status === 'todo'" :key="task.id">
            <div class="card-content">{{ task.description }}</div>
            <footer class="card-footer">
              <a class="card-footer-item">Done</a>
            </footer> 
          </div>
        </div>
      </div>
      <div class="column is-6">
        <h2 class="subtitle">Done</h2>
        <div class="todo">
          <div class="card" v-for="task in tasks" v-if="task.status === 'done'" :key="task.id">
            <div class="card-content">{{ task.description }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Home',
  data () {
    return {
      tasks: []
    }
  },
  mounted() {
    this.getTasks()
  },
  methods: {
    getTasks() {
      axios({
        method:'get',
        url:'http://127.0.0.1:8000/tasks/',
        auth: {
          username:'admin',
          password: 'admin'
        }
      }).then(response => this.tasks = response.data)
    }
  }
}
</script>
<style lang="scss">
.select,select {
  width: 100%;
}
.card {
  margin-bottom:20px;
}
.done {
  opacity: 0.3;
}
</style>
