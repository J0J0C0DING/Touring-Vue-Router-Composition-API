<script setup>
const props = defineProps(['event'])
</script>

<template>
    <div v-if="event">
        <p>Edit the event here</p>
    </div>
</template>
