<template>
  <div class="about">
    <h1>This is the Dashboard page</h1>
    <div>Email: {{ email }}</div>
    <div>Name: {{ name }}</div>
    <button @click="logout">Logout</button>
  </div>
</template>

<script>
import axios from 'axios'

axios.defaults.withCredentials = true
axios.defaults.baseURL = 'http://localhost:8000'



export default {
  data() {
    return {
      email: '',
      name:''
    }
  },
  mounted() {
    axios.get('/api/user')
      .then(response => {
        this.email = response.data.email,
        this.name = response.data.name
      })
      .catch(error=>{
        console.log(error.response);
      })
  },
  methods: {
    logout() {
      axios.post('api/logout')
      .then(response => {
        this.$router.push({ name: 'Home' })
      })
    }
  }
}
</script>
