<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h1 class="text-center">Hello {{username}}</h1>
      </div>
      <div class="card-body">
        <div v-if="notificationActive" class="alert alert-warning alert-dismissible fade show" role="alert">
          {{notification}}
          <button @click="closeAlert()" type="button" class="close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
        <div class="messages text-left">
          <div v-for="(item,index) in listMessage" :key="index">
           {{item}}
          </div>
        </div>
      </div>
      <div class="card-footer">
        <form @submit.prevent="sendMessage()">
          <div class="input-group mb-3">
  <input v-model="message" type="text" class="form-control" placeholder="Recipient's username" aria-label="Recipient's username" aria-describedby="button-addon2">
  <div class="input-group-append">
    <button @click.prevent="sendMessage()" class="btn btn-primary" type="button" id="button-addon2">Send Message</button>
  </div>
</div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import io from 'socket.io-client'
export default {
  name: 'Home',
  data () {
    return {
      message: '',
      username: this.$route.query.username,
      listMessage: [],
      socket: io('http://54.161.214.210:3000'),
      notificationActive: false,
      notification: ''
    }
  },
  methods: {
    sendMessage () {
      // this.socket.emit('key',value)
      // kita kirim /pancarkan message ke backend
      this.socket.emit('send-message', {
        username: this.username,
        message: this.message
      })
      this.message = ''
    },
    closeAlert () {
      this.notificationActive = false
    }
  },
  mounted () {
    this.socket.on('get-message', (payload) => {
      this.listMessage = [...this.listMessage, payload]
    })
    this.socket.on('get-notification', (notif) => {
      this.notificationActive = true
      this.notification = notif
    })
  }
}
</script>
<style scoped>
  .messages{
    min-height: 50vh;
  }
</style>
