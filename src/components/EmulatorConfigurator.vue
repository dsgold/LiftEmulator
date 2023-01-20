<template>
  <button @click="this.$emit('add-lift',null)">
    Add lift
  </button>
  <button @click="this.$emit('add-floor',null)">
    Add floor
  </button>
  <button @click="this.$emit('remove-lift',null)">
    Remove lift
  </button>
  <button @click="this.$emit('remove-floor',null)">
    Remove floor
  </button>
</template>

<script>
export default {
  name: 'EmulatorConfigurator',
  emits: ['add-lift','add-floor','remove-lift','remove-floor']
}
</script>

<style scoped>
</style>