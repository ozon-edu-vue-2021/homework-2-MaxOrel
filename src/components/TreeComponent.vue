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
<!--    <div :class="classNode" :href="items.type" @click="toggle">-->
<!--      {{items.name}}-->
<!--      <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>-->
<!--      <a v-if="isLink" :href="items.target">Перейти</a>-->
<!--    </div>-->

    <ul class="child-nodes" v-if="isOpen && isFolder">
      <tree-component
          v-for="(child,i) in items.contents"
          :key="`${child.name}-${i}`"
          :items="child"
          :bus="bus"
      />
    </ul>
  </li>

</template>

<script>
import DirectoryNode from "./DirectoryNode/DirectoryNode";
import FileNode from "./FileNode/FileNode";
import LinkNode from "./LinkNode/LinkNode";
export default {
  name: "TreeComponent",
  components: {LinkNode, FileNode, DirectoryNode},
  props: {
    items: {
      type: Object,
      default: ()=>({})
    },
    bus: {
      type: Object,
      default: ()=> ({}),
    }
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
    selected: function (event, el) {
      event.preventDefault();
      this.isSelected = !this.isSelected;
      this.getFullParentPath(el);
    },
    getFullParentPath: function (el){
      const pathName = `${this.getParentPath(el)}/${this.items.name}`
      this.bus.$emit('update-path', pathName);
    },
    getParentPath: function (currentElement) {
      const parrentTreeArray = this.loopParents(currentElement, 'tree');
      return parrentTreeArray
          .reverse()
          .map((element) => {
            return element.firstChild.textContent.trim()
            }
          )
          .join('/');
    },
    loopParents: function (el, parrentId) {
      let node = el;
      const arrayNodes = [];
      while (node != null) {
        node = node.parentNode;
        if(node.classList.contains('child-nodes')){
          const parentNode = node.previousElementSibling;
          // console.log(parentNode.textContent)
          if(parentNode.classList.contains('directory')){
            arrayNodes.push(parentNode)
          }
        }
        if(node === document.querySelector(`#${parrentId}`)){
          break;
        }
      }
      // console.log('click', arrayNodes);
      return arrayNodes;
    },
  },
  mounted() {
  }
}
</script>

<style scoped>
.node.selected {
  font-weight: bold;
}

.child-nodes {
  list-style: none;
}
</style>