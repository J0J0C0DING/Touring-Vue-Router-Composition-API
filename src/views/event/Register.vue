<script setup>
import { useRouter } from 'vue-router'
const props = defineProps(['event'])
const router = useRouter()

const register = () => {
    // If registration API call is successful
    // Push back to event details
    router.push({
        name: 'EventDetails',
        id: props.event.id,
    })
}
</script>
<template>
    <p>Register for the event here</p>
    <button @click="register">Register Me!</button>
</template>
