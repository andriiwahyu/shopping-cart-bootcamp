<template>
  <nav class="navbar px-5 py-3">
  <div class="container-fluid d-flex justify-content-between align-items-center">
    <h4 class="fw-bold">CartApp</h4>
    <div>
      <Button 
        text="Add Product" 
        color="btn-light"
        @func-click="$emit('show-add-form')" 
      />
    </div>
  </div>
</nav>
</template>

<script setup>
import Button from './Button.vue';

defineProps({
  totalCartItems: {
    type: Number,
    default: 0
  }
})

defineEmits(['add-product','show-add-form'])
</script>

<style scoped>
h4 {
  color: var(--white);
}
.navbar {
  background-color: var(--primary);
}
</style>