<template>
    <div>
        <input type="search"
            placeholder="search"
            required
            v-model="input"
            class="w-full px-4 py-3 border rounded-lg focus:outline-none focus:ring-2"
            v-debounce:400="() => emit('search', input)"
        >
    </div>
</template>

<script setup>
import { ref } from "vue";
import vueDebounce from "vue-debounce";

const input = ref('')
const emit = defineEmits(['search'])
const vDebounce = vueDebounce({ lock: true })

</script>