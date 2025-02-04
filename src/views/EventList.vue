<script setup>
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'
import { onMounted, ref, watch, computed } from 'vue'
import { RouterLink } from 'vue-router'

const props = defineProps(['page'])

const events = ref(null)
const totalEvents = ref(0)

const totalPages = ref(0)

const hasNextPage = computed(() => {
    totalPages.value = Math.ceil(totalEvents.value / 2)
    return props.page < totalPages.value
})

const fetchEvents = () => {
    EventService.getEvents(2, props.page)
        .then((response) => {
            events.value = response.data
            totalEvents.value = response.headers['x-total-count']
        })
        .catch((error) => {
            console.log(error)
        })
}

onMounted(() => {
    fetchEvents()
})

watch(
    () => props.page,
    () => {
        events.value = null
        fetchEvents()
    }
)
</script>

<template>
    <h1>Events for Good</h1>
    <div class="events">
        <EventCard v-for="event in events" :key="event.id" :event="event" />
        <div class="pagination">
            <RouterLink
                id="page_prev"
                :to="{ name: 'EventList', query: { page: page - 1 } }"
                rel="prev"
                v-if="page != 1"
            >
                &#60; Previous
            </RouterLink>
            <div v-else class="hidden"></div>
            <div class="number-nav">
                <RouterLink
                    :key="`nav-page-${n}`"
                    :id="`navigate_to_page_${n}`"
                    class="page-nav"
                    :class="{ active: page === n }"
                    v-for="n in totalPages"
                    :to="{ name: 'EventList', query: { page: n } }"
                >
                    {{ n }}
                </RouterLink>
            </div>
            <RouterLink
                id="page_next"
                :to="{ name: 'EventList', query: { page: page + 1 } }"
                rel="prev"
                v-if="hasNextPage"
            >
                Next &#62;
            </RouterLink>
            <div v-else class="hidden"></div>
        </div>
    </div>
</template>

<style scoped>
.events {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.pagination {
    display: flex;
    width: 290px;
}
.pagination a,
.pagination div.hidden {
    flex: 1;
    text-decoration: none;
    color: #2c3e50;
}

#page_prev {
    text-align: left;
}

#page_next {
    text-align: right;
}

.number-nav {
    flex: 1;
    display: flex;
}

.page-nav {
    &.active {
        color: green;
    }
}
</style>
