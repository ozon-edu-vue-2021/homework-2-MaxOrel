<template>
  <div id="app">
    <ul class="tree">
      <tree-component
          :items="json"
      />
    </ul>

  </div>
</template>

<script>
import data from '../public/static/data-slice.json'
import TreeComponent from "./components/TreeComponent";

export default {
  name: 'App',
  components: {TreeComponent},
  data() {
    return {
      json: data,
    }
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