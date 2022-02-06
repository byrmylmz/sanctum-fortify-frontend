<template>
  <form action="#" method="POST" @submit.prevent="login">
      <div>
        <input type="email" name="email" v-model="email" placeholder="email">
      </div>
      <div>
        <input type="password" name="password" v-model="password" placeholder="password">
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
      email: '',
      password: '',
    }
  },
  methods: {
    login() {
      // alert(`logging in ${this.email} with password: ${this.password}`)
      // this.$router.push({ name: 'Dashboard' })
      axios.get('/sanctum/csrf-cookie').then(response => {
          axios.post('api/login', {
            email: this.email,
            password: this.password,
          })
          .then(response => {
            console.log('Oturum acma basarili');
            this.$router.push({ name: 'Dashboard' })
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
