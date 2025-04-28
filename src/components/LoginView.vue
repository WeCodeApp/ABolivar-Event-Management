<template>
  <div class="min-h-screen w-full flex items-center justify-center">
    <div class="w-[400px] h-full align-center">
      <div class="bg-white rounded-xl shadow-xl p-8 border border-gray-100">
        <h2 class="text-2xl font-bold mb-12 text-center text-gray-800">Welcome Back</h2>
        <form @submit.prevent="handleLogin">
          <div class="mb-5">
            <label class="block text-gray-700 mb-2 font-medium" for="email">Email</label>
            <div class="relative">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 text-gray-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M16 12a4 4 0 10-8 0 4 4 0 008 0zm0 0v1.5a2.5 2.5 0 005 0V12a9 9 0 10-9 9m4.5-1.206a8.959 8.959 0 01-4.5 1.207"
                  />
                </svg>
              </div>
              <input
                v-model="form.email"
                type="email"
                id="email"
                class="w-full pl-10 pr-3 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-transparent text-black"
                placeholder="you@example.com"
                required
              />
            </div>
          </div>
          <div class="mb-12">
            <label class="block text-gray-700 mb-2 font-medium" for="password">Password</label>
            <div class="relative">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 text-gray-400"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"
                  />
                </svg>
              </div>
              <input
                v-model="form.password"
                type="password"
                id="password"
                class="w-full pl-10 pr-3 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-transparent text-black"
                placeholder="••••••••"
                required
              />
            </div>
          </div>
          <div>
            <p>-</p>
          </div>
          <button
            type="submit"
            class="w-full bg-gradient-to-r from-purple-600 to-blue-600 text-white py-3 px-4 rounded-lg hover:opacity-90 transition shadow-md font-medium text-lg"
          >
            Sign In
          </button>
        </form>
        <p>-</p>
        <div class="text-center text-sm text-gray-500">
          Sample account used:
          <div class="grid grid-cols-1 gap-2 mt-2 text-xs">
            <div class="bg-gray-50 p-2 rounded">bob@example.com</div>
            <div class="col-span-3 text-center mt-1">Password: password123</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'

interface LoginForm {
  email: string
  password: string
}

const form = reactive<LoginForm>({
  email: '',
  password: '',
})

const error = ref('')

const emit = defineEmits(['login'])

const handleLogin = () => {
  emit('login', {
    email: form.email,
    password: form.password,
  })
}
</script>
