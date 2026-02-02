<script setup lang="ts">
import SearchBar from '@/components/SearchBar.vue';
import SearchResultList from '@/components/SearchResultList.vue';
import { computed, onMounted, ref, watch } from 'vue';


const query = ref('')
const loading = ref(false)
const error = ref('')
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
        error.value = err
    } finally {
        loading.value = false
    }
}

onMounted(() => {
    fetchData()
})

</script>
<template>
    <div class="flex flex-col w-[500px] mx-auto">
        <div class="text-center mb-4">
            <h1 class="font-bold text-5xl mb-2">Search</h1>
            <p class="text-gray-500">Find what you're Looking for</p>
        </div>
        <SearchBar @search="handleSearch"/>
        <div v-if="!loading && filteredProducts.length" class="mt-4">
            <SearchResultList :products="filteredProducts" />
        </div>
    </div>
</template>