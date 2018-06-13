<template>
  <a href="#" :class="classes" @click="play(file)">{{ file.name }}</a>
</template>

<script>
export default {
  name: "file",

  props: [
    "file",
    "classes",
    "play"
  ]
}
</script>
