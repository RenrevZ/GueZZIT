<template>
  <HelloWorld />
</template>

<script>
import { defineComponent } from 'vue';
import HelloWorld from '../components/HelloWorld.vue';

export default defineComponent({
  name: 'HomeView',

  components: {
    HelloWorld,
  },
});
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Lilita+One&display=swap');
  body{
    font-family: 'Lilita One', cursive;
  }
</style>