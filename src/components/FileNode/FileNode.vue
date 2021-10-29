<template>
  <div
      ref="node"
      :class="classes"
      @click.stop="clickEvent($event, $refs.node)"
      @keyup.enter="clickEvent($event, $refs.node)"
      @keyup.prevent.space="clickEvent($event, $refs.node)"
      tabindex="0">
      <svg aria-hidden="true" focusable="false" role="img" class="fill-current fill-current--carbon" width="20px" height="20px" preserveAspectRatio="xMidYMid meet" viewBox="0 0 32 32" style="transform: rotate(360deg);"><path d="M25.7 9.3l-7-7c-.2-.2-.4-.3-.7-.3H8c-1.1 0-2 .9-2 2v24c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V10c0-.3-.1-.5-.3-.7zM18 4.4l5.6 5.6H18V4.4zM24 28H8V4h8v6c0 1.1.9 2 2 2h6v16z" fill="currentColor"></path><path d="M10 22h12v2H10z" fill="currentColor"></path><path d="M10 16h12v2H10z" fill="currentColor"></path></svg>
      {{item.name}}
  </div>
</template>


<script>
export default {
  name: "FileNode",
  props: {
    classNode: {
      type: Array,
      default: ()=>([]),
    },
    item: {
      type: Object,
      default: ()=>({})
    },
    activeElement: HTMLDivElement,
  },
  computed: {
    classes: function () {
      return [this.$refs.node === this.activeElement ? 'selected' : '', ...this.classNode]
    }
  },
  methods: {
    clickEvent: function(event, node) {
      this.$emit('selected-file', { event, node })
      this.$emit('bubble-path', {node: this.$refs.node, name: this.item.name});
    },
  }
}
</script>

<style scoped>

</style>