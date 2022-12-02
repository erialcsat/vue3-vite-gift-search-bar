<script setup>
import { ref, watch } from 'vue'
import { useFetch } from '@vueuse/core'

const searchTerm = ref('')

const products = ref([])

const getProductsTitle = products => {
  return products.map(product => product.title)
}

const findProducts = async term => {
  const url = `https://dummyjson.com/products/search?q=phone`
  const { isFetching, error, data } = await useFetch(url)
  const responsePayload = JSON.parse(data.value)
  products.value = getProductsTitle(responsePayload.products)
}

watch(searchTerm, newTerm => findProducts(newTerm))
</script>

<template>
  <div class="w-full h-full flex flex-col gap-5 justify-center items-center">
    <h1 class="text-4xl font-bold">Gift Search Bar</h1>
    <input type="text" class="p-2 border-2 border-gray-dark" v-model="searchTerm" placeholder="Start typing..." />
    <ul class="list-disc">
      <li v-for="product in products">{{ product }}</li>
    </ul>
  </div>
</template>
