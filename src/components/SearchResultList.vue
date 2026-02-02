
<template>
  <div>
    <SearchResultItem
      v-for="product in products"
      :key="product.id"
      :product="product"
    />
  </div>
</template>


<script setup>
import SearchResultItem from './SearchResultItem.vue'

const props = defineProps({
  products: {
    type: Array,
    default: () => []
  }
})

</script>


