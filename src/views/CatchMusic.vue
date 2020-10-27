<template>
  <div class="catch-music">
    <div>
      <h5 v-if="username">{{ username }}</h5>
      <h5 v-else>이름을 등록해주세요.</h5>
    </div>
    <div class="input-group mb-3">
      <input type="text" class="form-control" :placeholder="username" v-model="input">
      <div class="input-group-prepend">
        <input class="btn btn-primary" type="submit" value="Submit" @click="changeUsername">
      </div>
    </div>
    <ChatRoom/>
    <ChatRoom/>
    <ChatRoom/>
  </div>
</template>

<script>
// @ is an alias to /src
import ChatRoom from '@/components/ChatRoom.vue'

export default {
  name: 'CatchMusic',
  data() {
    return {
      'input': '',
      'username': ''
    }
  },
  components: {
    ChatRoom
  },
  methods: {
    changeUsername: function () {
      this.username = this.input
    }
  }
}
</script>
