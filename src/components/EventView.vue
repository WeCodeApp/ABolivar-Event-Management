<template>
  <div class="flex justify-center items-center">
    <div class="w-full max-w-4xl bg-white rounded-xl shadow-lg p-6 border border-gray-100">
      <h2 class="text-xl font-semibold mb-6 text-gray-800 flex items-center">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6 mr-2 text-purple-600"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M12 6v6m0 0v6m0-6h6m-6 0H6"
          />
        </svg>
        {{ editingEvent ? 'Update Event' : 'Add New Event' }}
      </h2>
      <form @submit.prevent="handleSubmit">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div>
            <label class="block text-gray-700 mb-2 font-medium" for="eventName">Event Name</label>
            <input
              v-model="eventForm.name"
              type="text"
              id="eventName"
              class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-transparent text-black"
              placeholder="Team meeting, Workshop, etc."
              required
            />
          </div>
          <div>
            <label class="block text-gray-700 mb-2 font-medium" for="eventDate"
              >Event Date & Time</label
            >
            <input
              v-model="eventForm.date"
              type="datetime-local"
              id="eventDate"
              class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-transparent text-black"
              required
            />
          </div>
        </div>
        <div class="mt-6 flex justify-end space-x-3">
          <button
            v-if="editingEvent"
            type="button"
            @click="cancelEdit"
            class="px-5 py-2 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300 transition font-medium"
          >
            Cancel
          </button>
          <button
            type="submit"
            class="px-5 py-2 bg-gradient-to-r from-purple-600 to-blue-600 text-white rounded-lg hover:opacity-90 transition shadow-md font-medium"
          >
            {{ editingEvent ? 'Update' : 'Add' }} Event
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, defineProps, defineEmits } from 'vue'

interface Event {
  id: number
  name: string
  date: string
  participants: any[]
}

const props = defineProps<{
  editingEvent: Event | null
}>()

const emit = defineEmits<{
  (e: 'save', eventData: { name: string; date: string }): void
  (e: 'cancel'): void
}>()

const eventForm = reactive({
  name: props.editingEvent?.name || '',
  date: props.editingEvent?.date || '',
})

const handleSubmit = () => {
  emit('save', {
    name: eventForm.name,
    date: eventForm.date,
  })
  eventForm.name = ''
  eventForm.date = ''
}

const cancelEdit = () => {
  emit('cancel')
  eventForm.name = ''
  eventForm.date = ''
}
</script>