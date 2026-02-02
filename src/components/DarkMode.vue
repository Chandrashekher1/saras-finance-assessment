<template>
    <div>
        <button @click="toggleDark" class="px-4 py-2 rounded dark:text-white cursor-pointer">
            {{ isDark ? '☀️' : '🌙' }}
        </button>
    </div>
</template>

<script setup>
import { ref } from 'vue'
const isDark = ref(false)
const toggleDark = () => {
    isDark.value = !isDark.value
    document.documentElement.classList.toggle('dark', isDark.value)
}

</script>