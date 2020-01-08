<template>
  <div id="app">
    <base-kanban />
  </div>
</template>

<script>
import Vue from 'vue';
import BaseKanban from '@/components/BaseKanban.vue';

export default Vue.extend({
  name: 'app',
  components: {
    BaseKanban,
  },
});
</script>

<style>
body {
    margin: 0;
  }
</style>
