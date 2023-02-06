<template>
  <div class="mt-5">
    <h2 class="mb-3">Cart ({{ totalItemCart }})</h2>
    <Table :rows="data" :columns="columns">
      <template #actions="{ row }">
        <Button 
          color="btn-danger"
          text="Delete one"
          @func-click="$emit('delete-item', row.id)"
        />
        <Button 
          color="btn-danger"
          text="Delete all"
          @func-click="$emit('delete-all', row.id)"
        />
      </template>
    </Table>  
    <div class="mt-2">
      <p>Total Price: Rp.{{ totalPrice }}</p>
    </div>

    <div class="mt-5" v-if="data.length > 0">
      <Button 
        color="btn-primary"
        text="Checkout"
        @func-click="$emit('checkout')"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Table from './Table.vue';
import Button from './Button.vue';

defineProps({
  data: {
    type: Array,
  },
  totalPrice: {
    type: Number,
  },
  totalItemCart: {
    type: Number,
    default: 0
  }
})
defineEmits(['delete-item', 'delete-all', 'checkout'])

const columns = ref( [
        { title: 'Name', field: 'name' },
        { title: 'Stock', field: 'stock' },
        { title: 'Price', field: 'price' },
      ])
</script>