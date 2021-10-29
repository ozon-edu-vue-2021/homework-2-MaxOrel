<template>
  <div id="app">
    <div>Путь к выбранному файлу/папке: {{path}}</div>
    <ul class="tree" id="tree">
      <tree-component
          :items="json" @bubble-path="handleUpdatePath"
      />
    </ul>

  </div>
</template>

<script>
import data from '../public/static/node_modules.json'
import TreeComponent from "./components/TreeComponent";

export default {
  name: 'App',
  components: {TreeComponent},
  data() {
    return {
      json: data,
      path: '',
    }
  },
  methods: {
    handleUpdatePath: function (data) {
      this.path = data;
    }
  }
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