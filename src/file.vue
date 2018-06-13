<template>
  <div :id="index">
    <a href="#" @click="play(file)">{{ file.name }}</a>
  </div>
</template>

<script>
export default {
  name: "file",

  props: [
    "file",
    "index",
    "play"
  ]
}
</script>
