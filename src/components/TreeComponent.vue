<template>
  <li>
    <directory-node v-if="items.type === 'directory'"
      :class="classNode"
      :item="items"
      :is-folder="isFolder"
      :is-open = "isOpen"
      @open-folder="toggle"
      @bubble-path="handleUpdatePath"
      @active-element-picked="handlePickedElement"
    />
    <file-node v-if="items.type === 'file'"
      :class="classNode"
      :item="items"
      :is-selected="isSelected"
      @selected-file="selected"
      @bubble-path="handleUpdatePath"
      :activeElement = "activeElement"
      @active-element-picked="handlePickedElement"
    />
    <link-node v-if="items.type === 'link'"
      :class="classNode"
      :item="items"
      :href="getLink"
      :is-selected="isSelected"
      @selected-file="selected"
      @bubble-path="handleUpdatePath"
      @active-element-picked="handlePickedElement"
    />
    <ul class="child-nodes" v-if="isOpen && isFolder">
      <tree-component
          v-for="(child,i) in items.contents"
          :key="`${child.name}-${i}`"
          :items="child"
          @bubble-path="handleUpdatePath"
          :activeElement = "activeElement"
          @active-element-picked="handlePickedElement"
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
  components: {FileNode,LinkNode, DirectoryNode},
  props: {
    items: {
      type: Object,
      default: ()=>({})
    },
    activeElement: HTMLDivElement,
    default: ()=>({})
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
        // 'selected': this.isSelected,
      }]
    },

  },
  methods: {
    toggle: function() {
      if (this.isFolder) {
        this.isOpen = !this.isOpen;
      }
    },
    handleUpdatePath: function (data) {
      this.$emit('bubble-path', data)
    },
    selected: function ({ event, node}) {
      event.preventDefault();
      // this.activeElement = node;
      // this.isSelected = !this.isSelected;

      this.$emit('active-element-picked', { event, node });
    },

    handlePickedElement ({ event, node }) {
      // this.activeElement = node;

      this.$emit('active-element-picked', { event, node });
    }
  },
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