<template>
  <div class="min-h-screen bg-gradient-to-b from-purple-50 to-blue-50">
    <header class="bg-gradient-to-r from-purple-600 to-blue-600 shadow-lg">
      <div class="container mx-auto px-4 py-5 flex justify-between items-center">
        <h1 class="text-2xl font-bold text-white flex items-center">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-7 w-7 mr-2"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
            />
          </svg>
          Event Manager
        </h1>
        <div v-if="currentUser" class="flex items-center">
          <div class="mr-4 text-white flex items-center">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 mr-1"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
              />
            </svg>
            <span>{{ currentUser.name }}</span>
          </div>
          <button
            @click="logout"
            class="px-4 py-2 bg-white text-purple-600 rounded-lg hover:bg-gray-100 transition shadow-md font-medium"
          >
            Logout
          </button>
        </div>
      </div>
    </header>

    <LoginForm v-if="!currentUser" @login="handleLogin" />

    <main v-else class="container mx-auto px-4 py-8">
      <EventView :editing-event="editingEvent" @save="saveEvent" @cancel="cancelEdit" />

      <div class="flex justify-center items-center">
        <div
          class="w-full max-w-4xl bg-white rounded-xl shadow-lg overflow-hidden border border-gray-100"
        >
          <div
            class="px-6 py-4 border-b border-gray-200 bg-gradient-to-r from-purple-50 to-blue-50"
          >
            <h2 class="text-xl font-semibold text-gray-800 flex items-center">
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
                  d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
                />
              </svg>
              Your Events
            </h2>
          </div>
          <div v-if="events.length === 0" class="p-16 text-center">
            <div class="flex justify-center mb-4">
              <div class="bg-gray-100 p-4 rounded-full">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-12 w-12 text-gray-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"
                  />
                </svg>
              </div>
            </div>
            <h3 class="text-xl font-medium text-gray-700 mb-2">No events available</h3>
            <p class="text-gray-500">Create your first event by filling out the form above!</p>
          </div>
          <ul v-else class="divide-y divide-gray-200">
            <li
              v-for="event in events"
              :key="event.id"
              class="hover:bg-gray-50 transition duration-150 ease-in-out"
            >
              <div class="p-6">
                <div class="flex flex-col md:flex-row md:justify-between md:items-center">
                  <div class="mb-4 md:mb-0">
                    <div class="flex items-center">
                      <div
                        class="bg-gradient-to-r from-purple-600 to-blue-600 w-2 h-10 rounded-full mr-3"
                      ></div>
                      <div>
                        <h3 class="text-lg font-medium text-gray-900">{{ event.name }}</h3>
                        <p class="text-gray-600 flex items-center mt-1">
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            class="h-4 w-4 mr-1"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke="currentColor"
                          >
                            <path
                              stroke-linecap="round"
                              stroke-linejoin="round"
                              stroke-width="2"
                              d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"
                            />
                          </svg>
                          {{ formatDate(event.date) }}
                        </p>
                      </div>
                    </div>
                    <div class="mt-2 flex items-center">
                      <span
                        class="bg-purple-100 text-purple-800 text-xs font-medium px-2.5 py-0.5 rounded-full flex items-center"
                      >
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          class="h-3 w-3 mr-1"
                          fill="none"
                          viewBox="0 0 24 24"
                          stroke="currentColor"
                        >
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"
                          />
                        </svg>
                        {{ event.participants.length }} participant(s)
                      </span>
                    </div>
                  </div>
                  <div class="flex flex-wrap gap-2">
                    <button
                      v-if="!isParticipating(event)"
                      @click="joinEvent(event)"
                      class="px-3 py-1 bg-green-500 text-white rounded-lg hover:bg-green-600 transition shadow flex items-center"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-4 w-4 mr-1"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M18 9v3m0 0v3m0-3h3m-3 0h-3m-2-5a4 4 0 11-8 0 4 4 0 018 0zM3 20a6 6 0 0112 0v1H3v-1z"
                        />
                      </svg>
                      Join
                    </button>
                    <button
                      v-else
                      @click="leaveEvent(event)"
                      class="px-3 py-1 bg-amber-500 text-white rounded-lg hover:bg-amber-600 transition shadow flex items-center"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-4 w-4 mr-1"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M13 7a4 4 0 11-8 0 4 4 0 018 0zM9 14a6 6 0 00-6 6v1h12v-1a6 6 0 00-6-6zM21 12h-6"
                        />
                      </svg>
                      Leave
                    </button>
                    <button
                      @click="editEvent(event)"
                      class="px-3 py-1 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition shadow flex items-center"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-4 w-4 mr-1"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"
                        />
                      </svg>
                      Edit
                    </button>
                    <button
                      @click="deleteEvent(event)"
                      class="px-3 py-1 bg-red-500 text-white rounded-lg hover:bg-red-600 transition shadow flex items-center"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-4 w-4 mr-1"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                        />
                      </svg>
                      Delete
                    </button>
                    <button
                      @click="toggleEventDetails(event)"
                      class="px-3 py-1 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300 transition flex items-center"
                    >
                      <svg
                        v-if="!expandedEvents.includes(event.id)"
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-4 w-4 mr-1"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M19 9l-7 7-7-7"
                        />
                      </svg>
                      <svg
                        v-else
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-4 w-4 mr-1"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M5 15l7-7 7 7"
                        />
                      </svg>
                      {{ expandedEvents.includes(event.id) ? 'Hide Details' : 'View Details' }}
                    </button>
                  </div>
                </div>

                <!-- Event Details -->
                <div
                  v-if="expandedEvents.includes(event.id)"
                  class="mt-6 bg-gradient-to-r from-purple-50 to-blue-50 p-4 rounded-lg border border-gray-200 expanded-details"
                >
                  <h4 class="font-medium mb-3 text-gray-800 flex items-center">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      class="h-5 w-5 mr-1 text-purple-600"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"
                      />
                    </svg>
                    Participants:
                  </h4>
                  <ul v-if="event.participants.length > 0" class="space-y-2">
                    <li
                      v-for="participant in event.participants"
                      :key="participant.id"
                      class="flex items-center bg-white p-2 rounded-lg shadow-sm"
                    >
                      <div
                        class="bg-gradient-to-r from-purple-600 to-blue-600 h-8 w-8 rounded-full flex items-center justify-center text-white font-medium"
                      >
                        {{ participant.name.charAt(0) }}
                      </div>
                      <div class="ml-2">
                        <div class="text-gray-800 font-medium">{{ participant.name }}</div>
                        <div class="text-gray-500 text-sm">{{ participant.email }}</div>
                      </div>
                    </li>
                  </ul>
                  <div v-else class="bg-white p-4 rounded-lg text-gray-500 text-center shadow-sm">
                    No participants yet
                  </div>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, computed, onMounted } from 'vue'
import LoginForm from '@/components/LoginView.vue'
import EventView from '@/components/EventView.vue'

interface User {
  id: number
  name: string
  email: string
  password: string
}

interface Event {
  id: number
  name: string
  date: string
  participants: User[]
}

const users: User[] = [
  { id: 1, name: 'John Doe', email: 'john@example.com', password: 'password123' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com', password: 'password123' },
  { id: 3, name: 'Bob Johnson', email: 'bob@example.com', password: 'password123' },
]

const currentUser = ref<User | null>(null)
const events = ref<Event[]>([])
const eventForm = reactive({
  name: '',
  date: '',
})
const editingEvent = ref<Event | null>(null)
const expandedEvents = ref<number[]>([])

onMounted(() => {
  const savedUser = localStorage.getItem('currentUser')
  if (savedUser) {
    currentUser.value = JSON.parse(savedUser)
  }

  events.value = [
    {
      id: 1,
      name: 'Team Building Workshop',
      date: '2023-12-15T14:00',
      participants: [users[0]],
    },
    {
      id: 2,
      name: 'Annual Conference',
      date: '2023-12-20T09:00',
      participants: [users[0], users[1]],
    },
  ]
})

const handleLogin = ({ email, password }: { email: string; password: string }) => {
  const user = users.find((u) => u.email === email && u.password === password)

  if (user) {
    currentUser.value = user
    localStorage.setItem('currentUser', JSON.stringify(user))
  }
}

const logout = () => {
  currentUser.value = null
  localStorage.removeItem('currentUser')
}

const saveEvent = ({ name, date }: { name: string; date: string }) => {
  if (editingEvent.value) {
    const index = events.value.findIndex((e) => e.id === editingEvent.value!.id)
    if (index !== -1) {
      events.value[index] = {
        ...events.value[index],
        name,
        date,
      }
    }
    editingEvent.value = null
  } else {
    const newEvent: Event = {
      id: Date.now(),
      name,
      date,
      participants: currentUser.value ? [currentUser.value] : [],
    }
    events.value.push(newEvent)
  }
}

const editEvent = (event: Event) => {
  editingEvent.value = event
  eventForm.name = event.name
  eventForm.date = event.date
}

const cancelEdit = () => {
  editingEvent.value = null
  eventForm.name = ''
  eventForm.date = ''
}

const deleteEvent = (event: Event) => {
  if (confirm(`Are you sure you want to delete "${event.name}"?`)) {
    events.value = events.value.filter((e) => e.id !== event.id)
  }
}

const joinEvent = (event: Event) => {
  if (currentUser.value) {
    const eventIndex = events.value.findIndex((e) => e.id === event.id)
    if (eventIndex !== -1) {
      const updatedParticipants = [...events.value[eventIndex].participants]
      updatedParticipants.push(currentUser.value)

      events.value[eventIndex] = {
        ...events.value[eventIndex],
        participants: updatedParticipants,
      }
    }
  }
}

const leaveEvent = (event: Event) => {
  if (currentUser.value) {
    const eventIndex = events.value.findIndex((e) => e.id === event.id)
    if (eventIndex !== -1) {
      const updatedParticipants = events.value[eventIndex].participants.filter(
        (p) => p.id !== currentUser.value!.id,
      )

      events.value[eventIndex] = {
        ...events.value[eventIndex],
        participants: updatedParticipants,
      }
    }
  }
}

const isParticipating = (event: Event): boolean => {
  if (!currentUser.value) return false
  return event.participants.some((p) => p.id === currentUser.value!.id)
}

const toggleEventDetails = (event: Event) => {
  if (expandedEvents.value.includes(event.id)) {
    expandedEvents.value = expandedEvents.value.filter((id) => id !== event.id)
  } else {
    expandedEvents.value.push(event.id)
  }
}

const formatDate = (dateString: string): string => {
  const options: Intl.DateTimeFormatOptions = {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit',
  }
  return new Date(dateString).toLocaleDateString(undefined, options)
}
</script>

<style scoped>
.min-h-screen {
  min-height: 100vh;
  width: 100%;
}

.container {
  max-width: 100%; /* Change from 1200px to 100% */
  width: 100%;
  margin: 0 auto;
  padding-left: 1rem;
  padding-right: 1rem;
}

#app {
  width: 100%;
  min-height: 100vh;
}

button {
  transition: all 0.2s ease;
}

button:hover {
  transform: translateY(-1px);
}

button:active {
  transform: translateY(0);
}

.expanded-details {
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 768px) {
  .grid-cols-1 > * {
    margin-bottom: 1rem;
  }

  button {
    padding-top: 0.5rem;
    padding-bottom: 0.5rem;
  }

  .flex-wrap {
    margin-top: 0.75rem;
  }
}

::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #94a3b8;
}

input:focus {
  outline: none;
  box-shadow: 0 0 0 2px rgba(147, 51, 234, 0.3);
  transition: box-shadow 0.2s ease;
}

.hover\:bg-gray-50:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

button {
  position: relative;
  overflow: hidden;
}

button::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 5px;
  height: 5px;
  background: rgba(255, 255, 255, 0.5);
  opacity: 0;
  border-radius: 100%;
  transform: scale(1, 1) translate(-50%, -50%);
  transform-origin: 50% 50%;
}

button:active::after {
  opacity: 1;
  transform: scale(20, 20) translate(-50%, -50%);
  transition:
    transform 0.6s,
    opacity 0.6s;
}

input[type='datetime-local'] {
  appearance: none;
  -moz-appearance: none;
  -webkit-appearance: none;
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(147, 51, 234, 0.4);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(147, 51, 234, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(147, 51, 234, 0);
  }
}

.bg-gray-100 {
  animation: pulse 2s infinite;
}
</style>
