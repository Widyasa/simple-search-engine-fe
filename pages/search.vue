<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import Navbar from "~/components/navbar.vue";
definePageMeta({
  layout: 'index-layout'
})
// Placeholder data for search results
const sampleResults = [
  { title: 'First result', url: 'https://example.com/1', snippet: 'This is the first result.' },
  { title: 'Second result', url: 'https://example.com/2', snippet: 'This is the second result.' },
  { title: 'Third result', url: 'https://example.com/3', snippet: 'This is the third result.' },
]

const route = useRoute()
const router = useRouter()
const query = ref(route.query.q || '')
const results = ref([])
const darkMode = ref(false)

onMounted(() => {
  performSearch()
})

const performSearch = () => {
  if (query.value) {
    // Filter sampleResults based on query
    results.value = sampleResults.filter(result =>
        result.title.toLowerCase().includes(query.value.toLowerCase()) ||
        result.snippet.toLowerCase().includes(query.value.toLowerCase())
    )
  } else {
    results.value = []
  }
}

const toggleDarkMode = () => {
  darkMode.value = !darkMode.value
}
</script>
<template>
  <div :class="darkMode ? 'dark' : ''">
    <div class=" p-8 bg-gray-50 dark:bg-gray-900 text-gray-900 dark:text-gray-100">
      <div class="max-w-4xl mx-auto">
        <!-- Search bar di halaman hasil pencarian -->
        <form @submit.prevent="performSearch" class="mb-8">
          <div class="flex items-center border border-gray-300 dark:border-gray-700 rounded-full px-4 py-2">
            <input
                v-model="query"
                type="text"
                class="w-full focus:outline-none text-lg text-gray-900 dark:text-gray-100 bg-transparent"
                placeholder="Search Google"
            />
            <button type="submit" class="ml-2">
              <svg
                  class="w-6 h-6 text-gray-500 dark:text-gray-300"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
              >
                <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
                ></path>
              </svg>
            </button>
          </div>
        </form>

        <!-- Toggle Dark Mode Button -->

        <!-- Menampilkan hasil pencarian -->
        <div v-if="results.length">
          <p class="text-gray-600 dark:text-gray-300 mb-4">Showing results for "{{ query }}"</p>
          <ul>
            <li
                v-for="(result, index) in results"
                :key="index"
                class="mb-6 border-b pb-4 border-gray-300 dark:border-gray-700"
            >
              <a :href="result.url" class="text-blue-600 dark:text-blue-400 text-lg hover:underline">
                {{ result.title }}
              </a>
              <p class="text-gray-500 dark:text-gray-400">{{ result.url }}</p>
              <p class="text-gray-700 dark:text-gray-300">{{ result.snippet }}</p>
            </li>
          </ul>
        </div>

        <div v-else>
          <p class="text-gray-600 dark:text-gray-300">No results found for "{{ query }}"</p>
        </div>
      </div>
    </div>
  </div>
</template>
