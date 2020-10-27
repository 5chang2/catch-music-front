<template>
  <div class="catch-music">
    <!-- <iframe :src="videoUrl" frameborder="0" class="iframe"></iframe> -->
    <iframe :src="videoUrl" frameborder="0" class="iframe" :class="{ none: !iframeBool, block: iframeBool}"></iframe>

    <div>
      <h5 v-if="user.username">{{ user.username }}</h5>
      <h5 v-else>이름을 등록해주세요.</h5>
    </div>
    <div class="input-group mb-3">
      <input type="text" class="form-control" :placeholder="user.username" v-model="userNameInput">
      <div class="input-group-prepend">
        <input class="btn btn-primary" type="submit" value="Submit" @click="changeUsername">
      </div>
    </div>
    <button @click="disconnect" v-if="status === 'connected'" class="btn btn-danger">disconnect</button>
    <button @click="getRandomMusic" v-if="user.isHost === true" class="btn btn-warning">random</button>
    <!-- <button @click="getRandomMusic" class="btn btn-warning">random</button> -->
    
    <button @click="connect" v-if="status === 'disconnected'" class="btn btn-primary">connect</button>

    <div v-if="status ==='connected'">
        <input type="text" v-model="message" @keyup.enter="sendMessage">
        <button type="button" class="btn btn-primary" @click="sendMessage">submit</button>
    </div>
    <div class="row">
      <div class="col-4">
        <p>users</p>
        <p v-for="(user,idx) in userList" :key="idx">{{ user.username }}</p>
      </div>
      <div class="col-8">
        <p v-for="(chat, idx) in chats" :key="idx" class="text-left"><span class="badge badge-secondary">{{ chat.author }}</span> : {{ chat.message }}</p>
      </div>
    </div>
    <!-- <ChatRoom/> -->
  </div>
</template>

<script>
// @ is an alias to /src
// import ChatRoom from '@/components/ChatRoom.vue'

export default {
  name: 'CatchMusic',
  data() {
    return {
      userNameInput: '',
      user: {
        username: '',
        isHost: false,
      },
      userList: [],
      message: '',
      logs: [],
      status: 'disconnected',
      chats: [],
      chat: {},
      nowMusic: {},
      iframeBool: false,
    }
  },
  components: {
    // ChatRoom
  },
  methods: {
    changeUsername: function () {
      this.user.username = this.userNameInput
    },
    connect() {
        this.socket = new WebSocket("ws://127.0.0.1:8000/ws/chat/22/")
        this.socket.onopen = () => {
            // 닉네임 입력하여 입장
            this.socket.send(JSON.stringify({
                action: 'enter',
                username: this.user.username,
            }))
            this.status = 'connected'

            this.socket.onmessage = (event)=>{
                console.log(event.data)
                // this.chats.push(JSON.parse(event.data))
                if (JSON.parse(event.data).action == 'message') {
                  this.chat.author = JSON.parse(event.data).author
                  this.chat.message = JSON.parse(event.data).message
                  if(this.nowMusic.title == JSON.parse(event.data).message && this.user.username == JSON.parse(event.data).author ) {
                    alert('정답!!')
                  }
                  this.chat.time = new Date()
                  this.chats.unshift(this.chat)
                  this.chat = {}
                }
                else if (JSON.parse(event.data).action == 'enter') {
                  this.user.isHost = JSON.parse(event.data).isHost
                  this.userList = JSON.parse(event.data).userList
                  // console.log(JSON.parse(event.data))
                }
                else if (JSON.parse(event.data).action == 'music') {
                  console.log(JSON.parse(event.data))
                  this.nowMusic = JSON.parse(event.data).music
                
                  // this.youtubeURL = 
                }
                // console.log(this.chats)
                // chatLog.value += `${message}\n` 
            }
        }
    },
    disconnect() {
      this.socket.close()
      this.status = 'disconnected'
      this.chats = []
      this.userList = []
      this.user.isHost = false
    },
    sendMessage() {
      this.socket.send(JSON.stringify({
          action: 'message',
          message: this.message,
          username: this.user.username,
      }))
      this.message = ''
    },
    getRandomMusic() {
      // console.log('random')
      this.nowMusic = {}
      // this.iframeBool = true
      this.socket.send(JSON.stringify({
          action: 'music'
      }))
    }
  },
   computed: {
    videoUrl () {
      const youtubeURL = this.nowMusic.youtube_url;
      return `https://www.youtube.com/embed/${youtubeURL}?autoplay=1`
    }
  },
}
</script>

<style scoped>
  .iframe {

  }
  .none {
    display: none;
  }
  .block {
    display: inline;
  }
</style>
