<template>
  <div id="app">
    <div>Путь к выбранному файлу/папке: {{path}}</div>
    <ul class="tree" id="tree">
      <tree-component
          :items="json" :bus="bus"
      />
    </ul>

  </div>
</template>

<script>
import data from '../public/static/node_modules.json'
import TreeComponent from "./components/TreeComponent";
import Vue from "vue";

export default {
  name: 'App',
  components: {TreeComponent},
  data() {
    return {
      json: data,
      path: '',
      bus: new Vue(),
    }
  },
  created() {
    this.bus.$on('update-path', (pathName) => {
      this.path = pathName;
    });
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<style scoped>
.tree,  .tree ul{
  list-style: none;
}
</style>