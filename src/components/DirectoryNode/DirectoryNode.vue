<template>
  <button
      ref="node"
      :class="classNode"
      @click="$emit('open-folder', $refs.node ); $emit('bubble-path', {node: $refs.node, name:item.name});">
      <svg aria-hidden="true" focusable="false" role="img" class="fill-current fill-current--carbon" width="20px" height="20px" preserveAspectRatio="xMidYMid meet" viewBox="0 0 32 32" style="transform: rotate(360deg);"><path d="M11.17 6l3.42 3.41l.58.59H28v16H4V6h7.17m0-2H4a2 2 0 0 0-2 2v20a2 2 0 0 0 2 2h24a2 2 0 0 0 2-2V10a2 2 0 0 0-2-2H16l-3.41-3.41A2 2 0 0 0 11.17 4z" fill="currentColor"></path></svg>
      {{item.name}}
  </button>
</template>

<script>
export default {
  name: "DirectoryNode",
  props: {
    classNode: {
      type: String,
      default: "",
    },
    item: {
      type: Object,
      default: ()=>({})
    },
    isFolder: {
      type: Boolean,
      default: false
    },
    isOpen: {
      type: Boolean,
      default: false
    },
  },
}
</script>

<style scoped>
.node.directory {
  background-color: transparent;
  border: none;
}
</style>