<template>
  <div class="home">
    <h1 class="title">Vuengo</h1>
  
    <hr>
    <div class="columns">
      <div class="column is-3 is-offset-3">
        <form v-on:submit.prevent="addTask">
          <h2 class="subtitle">Add task</h2>
          <div class="field">
            <label for="" class="label">Description</label>
            <div class="control">
              <input type="text" class="input" v-model="description">
            </div>
          </div>

          <div class="field">
            <label class="label">Status</label>
            <div class="control">
              <div class="select">
                <select v-model="status">
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
              <a class="card-footer-item" @click="setStatus(task.id, 'done')">Done</a>
            </footer> 
          </div>
        </div>
      </div>
      <div class="column is-6">
        <h2 class="subtitle">Done</h2>
        <div class="done">
          <div class="card" v-for="task in tasks" v-if="task.status === 'done'" :key="task.id">
            <div class="card-content">{{ task.description }}</div>
            <footer class="card-footer">
              <a class="card-footer-item" @click="setStatus(task.id, 'todo')">To do</a>
            </footer> 
          </div>
        </div>
      </div>
    </div>
    <SnackBox
      :snackbar = "snackbar"
      :text = "text"
      :snackcolor = "color"
    />
  </div>
  
</template>

<script>
import axios from 'axios'
import SnackBox from '@/components/SnackBox'
export default {
  name: 'Home',
  components: {
    SnackBox
  },
  data () {
    return {
      tasks: [],
      description: '',
      status: 'todo',
      snackbar: false,
      text: '',
      color: 'info',
    }
  },
  mounted() {
    this.getTasks()
  },
  methods: {
    addTask() {
      this.snackbar = false
      if(this.description) {
        axios({
          method:'post',
          url:'http://127.0.0.1:8000/tasks/',
          data:{
            description: this.description,
            status: this.status
          },
          auth: {
            username:'admin',
            password: 'admin'
          }
        }).then((response) => {
          let newTask = {
            'id': response.data.id,
            'description': response.data.description,
            'status': response.data.status,
          }

          this.tasks.push(newTask)
          this.description = ''
          this.status = 'todo'
          this.snackbar = true
          this.text = 'Still need to do'
          this.color = 'success'
        }).catch((error) => {
          console.log('error',error);
        })
      }
    },
    getTasks() {
      this.snackbar = false
      axios({
        method:'get',
        url:'http://127.0.0.1:8000/tasks/',
        auth: {
          username:'admin',
          password: 'admin'
        }
      }).then(response => {
        this.tasks = response.data
      })
    },
    setStatus(id, status) {
      this.snackbar = false
      let description = ''
      const task = this.tasks.filter(task => task.id == id)[0]
      description = task.description
      axios({
        method:'put',
        url:'http://127.0.0.1:8000/tasks/'+ id +'/',
        auth: {
          username:'admin',
          password: 'admin'
        },
        headers: {
          'Content-Type' : 'application/json',

        },
        data: {
          status: status,
          description: description
        }
      }).then((response) => {
        task.status = status
        if(status == 'todo'){
          this.snackbar = true
          this.text = 'Still need to do'
          this.color = 'error'
        }else{
          this.snackbar = true
          this.text = 'Finish the Job'
          this.color = 'info'
        }
      })
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
