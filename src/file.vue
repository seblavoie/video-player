<template>
  <div :id="index">
    <a href="#" @click="show()">{{ name }}</a>

    <div class="embed-responsive embed-responsive-16by9" v-show="visible">
      <video controls :src="path"></video>
    </div>

  </div>
</template>

<script>
export default {
  name: "file",

  data: function() {
    return {
      path: "",
      name: "",
      visible: false
    }
  },

  props: [
    "file",
    "index"
  ],

  mounted: function() {
    this.path = this.file.path
    this.name = this.file.name
    this.$nextTick(function() {
      var el = document.getElementById(this.index).querySelector("video")
      el.playbackRate = this.$parent.speed
    })
  },

  methods: {
    show: function() {
      this.visible = true
    }
  }
}
</script>
