<template>
  <a ref="node" :class="classNode" :href="target" @click="$emit('selected-file', $event, $refs.node)" >
    {{item.name}}
  </a>
</template>

<script>
export default {
  name: "LinkNode",
  props: {
    classNode: {
      type: String,
      default: "",
    },
    target: {
      type:String,
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