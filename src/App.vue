<template>
  <div id="app" class="page-view">
    <SearchView/>
  </div>
</template>

<script>
import SearchView from './views/SearchView.vue';
export default {
  name: 'App',
  components:{
    SearchView
  }
};
</script>

<style scoped>
.page-view {
  padding-bottom: 11px;
  margin: 30px;
}
</style>

