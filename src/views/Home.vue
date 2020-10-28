<template>
  <div class="home">
    <h1>CatchMusic</h1>
    <button type="button" class="btn btn-primary" @click="getRandomMusic">Random</button>
    <hr>
    <!-- <iframe :src="videoUrl" frameborder="0" class="iframe" :class="{ none: !iframeBool, block: iframeBool}"></iframe> -->
    <iframe :src="videoUrl" frameborder="0" class="iframe"></iframe>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Home',
  data () {
    return {
      videoId: '',
      iframeBool: false,
    }
  },
  components: {
    // HelloWorld
  },
  methods: {
    getRandomMusic: function () {
      this.iframeBool = true
      // axios.get('http://127.0.0.1:8000/musics/random/')
      axios.get('https://38a82ecdcd39.ngrok.io/musics/random/')
        .then(res => {
          console.log(res.data.youtube_url)
          this.videoId = res.data.youtube_url
        })
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
