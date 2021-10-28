<template>
  <div ref="node" :class="classNode" @click="$emit('open-folder', $refs.node )">
    {{item.name}}
    <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>
  </div>
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
    }
  },
}
</script>

<style scoped>

</style>