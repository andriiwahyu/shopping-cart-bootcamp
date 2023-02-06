<template>
  <Nav @add-product="addProduct" @show-add-form="showAddForm" />
  <main class="py-3 px-5">
    <AddForm
    v-if="addForm"
    v-model:name="addFormData.name"
    v-model:description="addFormData.description"
    v-model:price="addFormData.price"
    v-model:stock="addFormData.stock"
    @add-product="addProduct"
    @show-add-form="showAddForm"
    />
    <Products 
      :data="availableProducts" 
      @add-item="addOneItem" 
      @add-all="addAllItem"
    />
    <Cart 
      :data="availableCart" 
      :totalPrice="totalPriceCart"
      :totalItemCart="totalItemCart"
      @delete-item="deleteItem" 
      @delete-all="deleteAllItem"
      @checkout="checkout"
    />
  </main>
</template>

<script setup>
import Nav from './components/Nav.vue'
import Products from './components/Products.vue'
import Cart from './components/Cart.vue'
import AddForm from './components/AddForm.vue'

import { ref,computed,reactive } from 'vue';
import Swal from 'sweetalert2'

const products = ref([
  {     
    id: 1, 
    name: 'Indomie Goreng Rendang', 
    description: 'Masakan instan terenak di dunia',
    stock: 10,
    price: 3900 
  },
  {     
    id: 2, 
    name: 'Mie Gelas Rendang', 
    description: 'Mie instan khusus anak kosan',
    stock: 3,
    price: 1500 
  },
  {     
    id: 3, 
    name: 'Bakmie Mewah', 
    description: 'kalau anak kosan jangan macam2 deh',
    stock: 80,
    price: 10000 
  },
])

const availableProducts = computed(() => products.value.filter(product => product.stock > 0));

const cart = ref([])
const availableCart = computed(() => cart.value.filter(product => product.stock > 0));

const addForm = ref(false)
const showAddForm = () => {
  addForm.value = !addForm.value
}

const addFormData = reactive({
  id: Math.floor(Math.random() * 500),
  name: '',
  description: '',
  stock: 0,
  price: 0
})

const addProduct = () => {
  products.value.push(addFormData)
  addForm.value = false
}

const addOneItem = (id) => {
  const product = products.value.find(product => product.id === id)
  if (product.stock > 0) {
    product.stock -= 1
    const item = cart.value.find(item => item.id === id)
    if (item) {
      item.stock += 1
      item.price = item.stock * product.price
    } else {
      cart.value.push({
        id: product.id,
        name: product.name,
        stock: 1,
        price: product.price
      })
    }
  }
}

const addAllItem = (id) => {
  const product = products.value.find(product => product.id === id)
  if (product.stock > 0) {
    const item = cart.value.find(item => item.id === id)
    if (item) {
      item.stock += product.stock
      item.price = item.stock * product.price
    } else {
      cart.value.push({
        id: product.id,
        name: product.name,
        stock: product.stock,
        price: product.price
      })
    }

    product.stock = 0;
  }
}

const deleteItem = (id) => {
  const product = products.value.find(product => product.id === id)
  const item = cart.value.find(item => item.id === id)
  if (item) {
    product.stock += 1
    item.stock -= 1;
    item.price = item.stock * product.price
  }
}

const deleteAllItem = (id) => {
  const product = products.value.find(product => product.id === id)
  const item = cart.value.find(item => item.id === id)
  if (item) {
    product.stock += item.stock
    item.stock = 0;
    item.price = item.stock * product.price
  }
}

const totalItemCart = computed(() => {
  let total = 0;
  cart.value.forEach(item => {
    total += item.stock
  })
  return total
})

const totalPriceCart = computed(() => {
  let total = 0;
  cart.value.forEach(item => {
    total += item.price
  })
  return total
})

const checkout = () => {
  Swal.fire(
    'Want to checkout?',
    `Total Price: ${totalPriceCart.value}`,
    'question'
  ).then((result) => {
    if (result.isConfirmed) {
      Swal.fire(
        'Checkout Success!',
        'Thank you for shopping with us',
        'success'
      ).then(() => {
        location.reload()
      })
    }
  })
}

</script>
