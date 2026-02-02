<script setup lang="ts">
import DarkMode from '@/components/DarkMode.vue';
import Loader from '@/components/Loader.vue';
import Pagination from '@/components/Pagination.vue';
import SearchBar from '@/components/SearchBar.vue';
import SearchResultList from '@/components/SearchResultList.vue';
import { computed, onMounted, ref, watch } from 'vue';


const query = ref('')
const loading = ref(false)
const error = ref(false)
const products = ref([])

const pageSize = 10
const currentPage = ref(1)

const handleSearch = (search) => {
    query.value = search
}

const filteredProducts = computed(() => {
    return products.value.filter((product) => 
        product.title.toLowerCase().includes(query.value.trim().toLowerCase())||
        product.description.toLowerCase().includes(query.value.trim().toLowerCase())
    )
})

const totalPages = computed(() => (
    Math.ceil(filteredProducts.value.length / pageSize)
))

const start = computed(() => (
    (currentPage.value - 1) * pageSize
))

const end = computed(() => (
    start.value + pageSize
))

const paginatedProducts = computed(() => {
    return filteredProducts.value.slice(start.value, end.value)
})

const handlePageChange = (page) => {
    currentPage.value = page
}
const fetchData = async () => {
    try {
        loading.value = true
        const response = await fetch(`https://dummyjson.com/products?limit=100`)
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
    <div class="flex md:justify-between dark:bg-black dark:text-white min-h-screen">
        <div class="flex flex-col md:w-[500px] w-full lg:w-[700px] md:mx-auto px-8">
        <div class="text-center mb-4 flex justify-center md:gap-8 items-center">
             <h1 class="font-bold md:text-5xl text-3xl mb-2 dark:text-white">Search</h1>
            <DarkMode />
        </div>
        <SearchBar @search="handleSearch"/>
        <div class="mx-10">
            <Pagination v-if="totalPages>1" :totalPages="totalPages" :currentPage="currentPage" @changePage="handlePageChange"/>
        </div>
        <div v-if="loading" class="mt-4">
            <Loader />
        </div>

        <div v-if="!loading && paginatedProducts.length" class="mt-4">
            <SearchResultList :products="paginatedProducts" />
        </div>

        <div v-if="!loading && !paginatedProducts.length && !error" class="flex justify-center mt-6">
        <p class="text-gray-400 italic">No search results found.</p>
      </div>

      <div v-if="error" class="flex justify-center mt-6">
        <p class="text-red-400 italic">Error fetching results.</p>
      </div>
    </div>
    
    
    </div>
</template>