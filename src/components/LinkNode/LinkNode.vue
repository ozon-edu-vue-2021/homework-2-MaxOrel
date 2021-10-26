<template>
  <a :class="classNode" :href="target" @click="$emit('selected-file', $event)" >
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