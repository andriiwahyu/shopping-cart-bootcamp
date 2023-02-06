<template>
  <div class="mt-2" > 
    <div class="mb-3 d-flex justify-content-between align-items-center">
      <h2>Products</h2>
    </div>
    <Table :rows="data" :columns="columns">
      <template #actions="{ row }">
        <Button 
          color="btn-transparent"
          text="Add one"
          @func-click="$emit('add-item', row.id)"
        />
        <Button 
          color="btn-transparent"
          text="Add all"
          @func-click="$emit('add-all',row.id)"
        />
      </template>
    </Table>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Table from './Table.vue';
import Button from './Button.vue';

defineProps(['data'])
defineEmits(['add-item', 'add-all'])

const columns = ref( [
        { title: 'Name', field: 'name' },
        { title: 'Description', field: 'description' },
        { title: 'Stock', field: 'stock' },
        { title: 'Price', field: 'price' },
      ])
</script>