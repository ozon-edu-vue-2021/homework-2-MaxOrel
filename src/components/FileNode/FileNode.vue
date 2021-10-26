<template>
  <div :class="classNode" @click="$emit('selected-file', $event)">
    {{item.name}}
  </div>
</template>


<script>
export default {
  name: "FileNode",
  props: {
    classNode: {
      type: String,
      default: "",
    },
    item: {
      type: Object,
      default: ()=>({})
    }
  },
}
</script>

<style scoped>

</style>