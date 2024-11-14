<script setup>
defineProps({
    status: String,
});
</script>
<template>
    <div v-if="status" class="mb-4 text-sm font-medium text-green-500">
        {{ status }}
    </div>
</template>
