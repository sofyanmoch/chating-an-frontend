<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
</template>
<script>
import io from 'socket.io-client'
export default {
  data () {
    return {
      socket: io('http://localhost:3000')
    }
  }
}
</script>
