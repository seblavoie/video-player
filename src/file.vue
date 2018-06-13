<template>
  <a href="#" :class="classes" @click="play(index)">{{ file.name }}</a>
</template>

<script>
export default {
  name: "file",

  props: [
    "file",
    "classes",
    "index",
    "play"
  ]
}
</script>
