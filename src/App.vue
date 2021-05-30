<template>
  <Slideshow account="niftypicture" />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Slideshow from './components/Slideshow.vue';

export default defineComponent({
  name: 'App',
  components: {
    Slideshow,
  },
});
</script>

<style>
html, body {
  background: #000;
  height: 100%;
  margin: 0;
  padding: 0;
}
.slideshow {
  height: 100%;
}
#app {
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  height: 100%;
  text-align: center;
}
</style>
