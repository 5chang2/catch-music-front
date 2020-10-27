<template>
  <div class="create-music">

    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="singer">singer</span>
      </div>
      <input type="text" class="form-control" placeholder="BTS, BlackPink">
    </div>

    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="title">title</span>
      </div>
      <input type="text" class="form-control" placeholder="dynamite, 마지막처럼" >
    </div>

    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="basic-addon3">https://youtu.be/</span>
      </div>
      <input type="text" class="form-control" id="basic-url" >
    </div>

    <button type="button" class="btn btn-primary btn-lg btn-block">submit</button>

  </div>

  

  
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    // HelloWorld
  }
}
</script>
