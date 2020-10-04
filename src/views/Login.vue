<template>
    <div>
        <div class="card-body">
            <form @submit.prevent="onLogin()">
                <div class="form-group">
                    <label for="exampleInputEmail1">Email address</label>
                    <input v-model="username" type="text" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                </div>
                <div class="form-group">
                    <label for="exampleInputPassword1">Password</label>
                    <input v-model="password" type="password" class="form-control" id="exampleInputPassword1">
                </div>
                <button type="submit" class="btn btn-primary">Submit</button>
            </form>
        </div>
    </div>
</template>
<script>
import io from 'socket.io-client'
export default {
  name: 'Login',
  data () {
    return {
      socket: io('http://54.161.214.210:3000'),
      username: '',
      password: ''
    }
  },
  methods: {
    onLogin () {
      this.socket.emit('notification', this.username)
      this.$router.push({
        path: '/home',
        query: {
          username: this.username
        }
      })
    }
  }
}
</script>
