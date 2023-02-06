<template>
  <button
    :type="type"
    :class="`btn ${color} btn-sm shadow-sm py-1 px-2`"
    @click="$emit('func-click')"
  >
    {{ text }}
  </button>
</template>

<script setup>
defineProps({
  text: {
    type: String,
  },
  color: {
    type: String,
  },
  type: {
    type: String,
    default: 'button',
  },
})

defineEmits(['func-click'])
</script>

<style scoped>
.btn {
  border-radius: 4px !important;
}
</style>