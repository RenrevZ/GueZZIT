<template>
  <MainGame />
</template>

<script>
import { defineComponent } from 'vue';
import MainGame from '@/components/MainGame.vue'

export default defineComponent({
  name: 'HomeView',
  components: {
      MainGame
  },
});
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Lilita+One&display=swap');
  body{
    font-family: 'Lilita One', cursive;
  }
</style>