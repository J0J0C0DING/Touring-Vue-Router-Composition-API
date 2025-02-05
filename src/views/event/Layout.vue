<script setup>
import EventService from '@/services/EventService.js'
import { computed, onMounted, ref } from 'vue'
import { RouterView } from 'vue-router'

const props = defineProps(['id'])

const event = ref('')
const id = computed(() => props.id)
onMounted(() => {
    EventService.getEvent(id.value)
        .then((response) => {
            event.value = response.data
        })
        .catch((error) => {
            console.log(error)
        })
})
</script>

<template>
    <div v-if="event">
        <h1>{{ event.title }}</h1>
        <div id="nav">
            <!-- Because these routes are nest routes, the :id param will be passed and reflected in the URL when the links are clicked -->
            <router-link :to="{ name: 'EventDetails' }">Details</router-link>
            |
            <router-link :to="{ name: 'EventRegister' }">Register</router-link>
            |
            <router-link :to="{ name: 'EventEdit' }">Edit</router-link>
        </div>

        <RouterView :event="event" />
    </div>
</template>
