<template>
  <PanelProgressbar></PanelProgressbar>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import PanelProgressbar from './components/PanelProgressbar.vue';
export default {
  name: 'App',
  components: { PanelProgressbar },
};
</script>
<style scoped>

</style>
