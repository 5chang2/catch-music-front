<template>
  <div>
      <h1>test</h1>
      <button @click="disconnect" v-if="status === 'connected'" class="btn btn-danger">disconnect</button>
      <button @click="connect" v-if="status === 'disconnected'" class="btn btn-primary">connect</button>

      <div v-if="status ==='connected'">
          <input type="text" v-model="message">
          <button type="button" class="btn btn-primary" @click="sendMessage">submit</button>
      </div>
      <p>{{ chats }}</p>
  </div>
</template>

<script>

export default {
  name: 'Test',
  data () {
    return {
      message: '',
      logs: [],
      status: 'disconnected',
      chats: [],
    }
  },
  methods: {
    connect() {
        this.socket = new WebSocket("ws://127.0.0.1:8000/ws/chat/asdf/")
        this.socket.onopen = () => {
            // this.logs.push({
                //     event: 'connect',
            //     data: 'test'
            // })
            this.socket.send(JSON.stringify({
                action: 'enter',
                username: 'change',
            }))
            this.status = 'connected'
            // this.socket.onmessage
            this.socket.onmessage = (event)=>{
                console.log(event.data)
                this.chats.push(JSON.parse(event.data))
                console.log(this.chats)
                // chatLog.value += `${message}\n` 
            }
        }
    },
    disconnect() {
        this.socket.close()
        this.status = 'disconnected'
    },
    sendMessage() {
        // const context = JSON.stringify({
        //     action: 'message',
        //     message: this.message,
        //     username: 'change',
        // })
        // console.log(context)
        this.socket.send(JSON.stringify({
            action: 'message',
            message: this.message,
            username: 'change',
        }))
        this.message = ''
    }
  },
  computed: {
    videoUrl () {
      const videoId = this.videoId;
      return `https://www.youtube.com/embed/${videoId}?autoplay=1`
    }
  },
  // mounted() {
  //   videoUrl()
  // }
}
</script>

<style>

</style>