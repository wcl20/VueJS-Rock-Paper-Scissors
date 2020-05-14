<template>
  <img :src="require('@/assets/' + move + '.png')"/>
</template>

<script>
export default {
  // Component name
  name: "Move",
  props: {
    move: String
  }
}
</script>

<style scoped>
img {
  width: 100px;
  height: 100px;
  margin: 0 20px;
  border-radius: 50%;
  background-color: black;
}

img:hover {
  box-shadow: 0 0 0 3px white, 0 0 0 7px black;
}
</style>
