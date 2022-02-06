<template>
  <form action="#" method="POST" @submit.prevent="register">
      <div>
        <input type="name" name="name" v-model="name" placeholder="name">
      </div>
      <div>
        <input type="email" name="email" v-model="email" placeholder="email">
      </div>
      <div>
        <input type="password" name="password" v-model="password" placeholder="password">
      </div>
      <div>
        <input type="password" name="password_confirmation" v-model="password_confirmation" placeholder="password_confirmation">
      </div>
      <div>
        <button type="submit">Login</button>
      </div>
    </form>
</template>

<script>
import axios from 'axios'

axios.defaults.withCredentials = true
axios.defaults.baseURL = 'http://localhost:8000'

export default {
  name: 'Home',
  data() {
    return {
      name:'',
      email: '',
      password: '',
      password_confirmation: ''
    }
  },
  methods: {
    register() {
      axios.get('/sanctum/csrf-cookie').then(response => {
          axios.post('api/register', {
            name: this.name,
            email: this.email,
            password: this.password,
            password_confirmation: this.password_confirmation,
          })
          .then(response => {
            console.log('Kullanici kaydi basarili');
            this.$router.push({ name: 'Home' })
          })
          .catch(error => {
            console.log(error.response);
            alert(error.response.data.message);
          })
      }).then(response=>{
        console.log('csrf token olusturuldu');
      })
      .catch(error=>{
         console.log(error.response);
      });
    }
  }
}
</script>
