<template>
  <ProgressBar></ProgressBar>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import ProgressBar from './components/ProgressBar.vue';
export default {
  name: 'App',
  components: { ProgressBar },
};
</script>
<style scoped>

</style>
