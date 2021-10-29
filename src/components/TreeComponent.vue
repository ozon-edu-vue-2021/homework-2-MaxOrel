<template>
  <li>
    <directory-node v-if="items.type === 'directory'"
      :class="classNode"
      :item="items"
      :is-folder="isFolder"
      :is-open = "isOpen"
      @open-folder="toggle"
    />
    <file-node v-if="items.type === 'file'"
      :class="classNode"
      :item="items"
      :is-selected="isSelected"
      @selected-file="selected"
    />
    <link-node v-if="items.type === 'link'"
      :class="classNode"
      :item="items"
      :href="getLink"
      :is-selected="isSelected"
      @selected-file="selected"
    />
    <ul class="child-nodes" v-if="isOpen && isFolder">
      <tree-component
          v-for="(child,i) in items.contents"
          :key="`${child.name}-${i}`"
          :items="child"
          @bubble-path="handleUpdatePath"
      />
    </ul>
  </li>

</template>

<script>
import DirectoryNode from "./DirectoryNode/DirectoryNode";
import FileNode from "./FileNode/FileNode";
import LinkNode from "./LinkNode/LinkNode";
import {CLASS_CHILD_NODE, CLASS_PARENT_NODE, CLASS_ROOT_TREE} from "../constans";
export default {
  name: "TreeComponent",
  components: { FileNode, LinkNode, DirectoryNode},
  props: {
    items: {
      type: Object,
      default: ()=>({})
    },
  },
  data: function() {
    return {
      isOpen: false,
      isSelected: false,
    };
  },
  computed: {
    isFolder: function() {
      return Boolean(this.items.contents && this.items.contents.length);
    },
    isLink: function() {
      return Boolean(this.items.type === 'link' && this.items.target.length);
    },
    getLink: function (){
      return this.items.target;
    },
    classNode: function () {
      return ['node', this.items.type, {
        'selected': this.isSelected,
      }]
    },
  },
  methods: {
    toggle: function(el) {
      if (this.isFolder) {
        this.isOpen = !this.isOpen;
        this.getFullParentPath(el);
      }
    },
    handleUpdatePath: function (data) {
      this.$emit('bubble-path', data)
    },
    selected: function (event, el) {
      event.preventDefault();
      this.isSelected = !this.isSelected;
      this.getFullParentPath(el);
    },
    getFullParentPath: function (el){
      console.log(el)
      const pathName = `${this.getParentPath(el)}/${this.items.name}`
      this.$emit('bubble-path', pathName);
    },
    getParentPath: function (currentElement) {
      const parrentTreeArray = this.loopParents(currentElement, CLASS_ROOT_TREE);
      return parrentTreeArray
          .reverse()
          .map((element) => {
            return element.textContent.trim()
            }
          )
          .join('/');
    },
    loopParents: function (el, parrentId) {
      let node = el;
      const arrayNodes = [];
      while (node != null) {
        node = node.parentNode;
        if(node.classList.contains(CLASS_CHILD_NODE)){
          const parentNode = node.previousElementSibling;
          // console.log(parentNode.textContent)
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
  },
  mounted() {
  }
}
</script>

<style scoped>
.node {
  display: flex;
  align-items: center;
  gap: 5px;
}
.node.selected {
  font-weight: bold;
}

.child-nodes {
  list-style: none;
}
</style>