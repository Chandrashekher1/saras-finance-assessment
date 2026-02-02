<template>
    <div>
        <input type="search"
            placeholder="search"
            required
            v-model="input"
            @input="emit('search', input)"
            class="w-full px-4 py-3 border rounded-lg focus:outline-none focus:ring-2"
        >
    </div>
</template>

<script setup>
import { ref } from "vue";
const input = ref('')

const emit = defineEmits(['search'])

</script>