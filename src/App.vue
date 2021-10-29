<template>
  <div id="app">
    <div>Путь к выбранному файлу/папке: {{path}}</div>
    <ul class="tree" id="tree">
      <tree-component
          :items="json" @bubble-path="handleUpdatePath" @active-element-picked="handlePickedElement" :activeElement="activeElement"
      />
    </ul>

  </div>
</template>

<script>
import data from '../public/static/node_modules.json'
import TreeComponent from "./components/TreeComponent";
import {CLASS_CHILD_NODE, CLASS_PARENT_NODE, CLASS_ROOT_TREE} from "./constans";

export default {
  name: 'App',
  components: {TreeComponent},
  data() {
    return {
      json: data,
      path: '',
      activeElement: null,
    }
  },
  methods: {
    handlePickedElement: function ({node}) {
      this.activeElement = node;
    },

    handleUpdatePath: function ({node, name}) {
      const pathName = this.getFullParentPath(node, name)
      this.path = pathName;
    },

    getFullParentPath: function (el, currentName){
      return `${this.getParentPath(el)}/${currentName}`
    },
    getParentPath: function (currentElement) {
      const parrentTreeArray = this.loopParents(currentElement, CLASS_ROOT_TREE);
      return parrentTreeArray
          .reverse()
          .map(element => element.textContent.trim())
          .join('/');
    },
    loopParents: function (el, parrentId) {
      let node = el;
      const arrayNodes = [];
      while (node != null) {
        node = node.parentNode;
        if(node.classList.contains(CLASS_CHILD_NODE)){
          const parentNode = node.previousElementSibling;
          if(parentNode.classList.contains(CLASS_PARENT_NODE)){
            arrayNodes.push(parentNode)
          }
        }
        if(node === document.querySelector(`#${parrentId}`)){
          break;
        }
      }
      return arrayNodes;
    },
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