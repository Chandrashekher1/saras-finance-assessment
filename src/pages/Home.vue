<script setup lang="ts">
import SearchBar from '@/components/SearchBar.vue';
import SearchResultList from '@/components/SearchResultList.vue';
import { computed, onMounted, ref } from 'vue';


const query = ref('')
const loading = ref(false)
const error = ref(false)
const products = ref([])

const handleSearch = (search) => {
    query.value = search
}

const filteredProducts = computed(() => {
    return products.value.filter((product) => 
        product.title.toLowerCase().includes(query.value.trim().toLowerCase())||
        product.description.toLowerCase().includes(query.value.trim().toLowerCase())
    )
})

const fetchData = async () => {
    try {
        loading.value = true
        const response = await fetch(`https://dummyjson.com/products`)
        const data = await response.json()
        products.value = data.products
    } catch (err) {
        error.value = true
    } finally {
        loading.value = false
    }
}

onMounted(() => {
    fetchData()
})

</script>
<template>
    <div class="flex flex-col md:w-[500px] w-full lg:w-[700px] md:mx-auto px-8">
        <div class="text-center mb-4">
            <h1 class="font-bold md:text-5xl text-3xl mb-2">Search</h1>
            <p class="text-gray-500">Find what you're Looking for</p>
        </div>
        <SearchBar @search="handleSearch"/>
        <div v-if="loading" class="mt-4">
            <Loader />
        </div>

        <div v-if="!loading && filteredProducts.length" class="mt-4">
            <SearchResultList :products="filteredProducts" />
        </div>

        <div v-if="!loading && !filteredProducts.length && !error" class="flex justify-center mt-6">
        <p class="text-gray-400 italic">No search results found.</p>
      </div>

      <div v-if="error" class="flex justify-center mt-6">
        <p class="text-red-400 italic">Error fetching results.</p>
      </div>
    </div>
</template>