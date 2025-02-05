<script setup>
const props = defineProps(['event'])
</script>

<template>
    <div v-if="event">
        <p>Register for the event</p>
    </div>
</template>
