<template>
  <div class="create-music">

    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="singer">singer</span>
      </div>
      <input type="text" class="form-control" placeholder="BTS, BlackPink" v-model="singer">
    </div>

    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="title">title</span>
      </div>
      <input type="text" class="form-control" placeholder="dynamite, 마지막처럼" v-model="title">
    </div>

    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="basic-addon3">https://youtu.be/</span>
      </div>
      <input type="text" class="form-control" id="basic-url" v-model="youtube_url">
    </div>

    <button type="button" class="btn btn-primary btn-lg btn-block" @click="createMusic">submit</button>

  </div>

  

  
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios';

export default {
  name: 'CreateMusic',
  data () {
    return {
      'singer': '',
      'title': '',
      'youtube_url': '',
    }
  },
  components: {
    // HelloWorld
  },
  methods: {
    createMusic: function () {
      const data = {
        singer: this.singer,
        title: this.title,
        youtube_url: this.youtube_url
      }
      axios.post('http://127.0.0.1:8000/musics/', data)
        .then(res => {
          console.log(res)
          if (res.status == 200) {
            alert('등록완료')
            this.singer = ''
            this.title = ''
            this.youtube_url = ''
          }
          else {
            alert('실패')
          }
        })
    }
  }
}
</script>
