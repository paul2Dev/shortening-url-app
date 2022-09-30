<script setup>
import { ref } from 'vue'
import { useClipboard, usePermission } from '@vueuse/core'
import axios from 'axios'

const url = ref('')
const shortUrl = ref('')  

const { text, isSupported, copy, copied } = useClipboard()
const permissionRead = usePermission('clipboard-read')
const permissionWrite = usePermission('clipboard-write')

const shorthenUrl = () => {
  axios.get('https://api.shrtco.de/v2/shorten?url=' + url.value)
    .then(response => {
      shortUrl.value = 'https://' + response.data.result.short_link
    })
    .catch(error => {
      shortUrl.value = error.response.data.error
    })
}

</script>

<template>

  <div class="container mx-auto h-screen flex items-center justify-center">
    <div class="flex flex-col bg-slate-800 text-green-500 p-10">
      <div class="text-center text-xl">
        <h1>Shorthen your URL</h1>
      </div>
      <div class="py-4">
        <input @keypress.enter="shorthenUrl" class="text-slate-800 text-center p-2 rounded-lg focus:outline-none focus:ring focus:ring-green-500" type="text" v-model="url" placeholder="enter your url" autofocus>
      </div>
      <div v-if="shortUrl" class="text-center flex flex-col items-center justify-center">
        <a :href="shortUrl" target="_blank">{{ shortUrl }}</a>
        <button class="mt-4 rounded w-20 bg-green-500 text-white p-1" @click="copy(url)">
          <!-- by default, `copied` will be reset in 1.5s -->
          <span v-if='!copied'>Copy</span>
          <span v-else>Copied!</span>
        </button>
      </div>
    </div>
  </div>
  
</template>

<style scoped>

</style>
