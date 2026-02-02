<template>
  <div class="mb-4 w-full hover:bg-gray-100 dark:hover:bg-white/20 hover:shadow-md hover:rounded-lg transition-all duration-200">
    <div class="border border-gray-200 dark:border-white/20 rounded-lg p-4 " >
      <div class="flex gap-4 cursor-pointer" @click="toggle">
        <img :src="product.thumbnail" alt="product image"loading="lazy" class="w-16 h-16 object-cover">
      <div >
        <h1 class="font-semibold md:text-lg text-base">{{ product.title }}</h1>
        <div class="mt-2">
          <span class="text-gray-500 mr-2 md:mr-4 dark:border-white/20 border border-gray-200 rounded-lg p-1 text-sm">{{ product.category ? product.category.toLowerCase() : 'N/A' }}</span>
        </div>
      </div>
      </div>
       <Transition
        enter-active-class="transition-all md:duration-500 ease-out"
        leave-active-class="transition-all md:duration-500 ease-in"
        enter-from-class="opacity-0 max-h-0"
        enter-to-class="opacity-100 max-h-40"
        leave-from-class="opacity-100 max-h-40"
        leave-to-class="opacity-0 max-h-0"
      >
      <div v-if="expanded" class="mt-4 ml-20 cursor-pointer ">
        <h1 class="font-semibold">₹{{ product.price ? product.price : 'N/A' }}</h1>
        <p class="text-gray-500">{{ product.description ? product.description : 'N/A' }}</p>
    </div>
    </Transition>
    </div>
    
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  product: {
    type: Object,
  }
})
const expanded = ref(false)

const toggle = () => {
  expanded.value = !expanded.value
}

</script>


