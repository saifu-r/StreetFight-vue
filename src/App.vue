<template>
  <div class="app">
    <gameComponent/>
  </div>

</template>

<script lang="ts">
import { defineComponent } from 'vue';
import gameComponent from './components/gameComponent.vue';

export default defineComponent({
  name: 'App',
  components: {gameComponent}
});
</script>

<style>

</style>
