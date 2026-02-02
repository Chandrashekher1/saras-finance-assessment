<template>
    <div class="flex items-center justify-center mt-6">
        <button :class="`px-2 py-1 border rounded-md mx-1 cursor-pointer hover:bg-gray-300 dark:border-white/20 dark:hover:bg-white/20 dark:text-white ${currentPage === page ? 'bg-gray-400 dark:bg-white/20' : ''}`" v-for="page in totalPages" @click="$emit('changePage',page)">{{page}}</button>
    </div>
</template>

<script setup>

defineProps({
    currentPage: Number,
    totalPages: Number
})

const emit = defineEmits(['changePage'])
</script>