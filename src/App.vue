<script setup>
import { ref } from 'vue'

import Logo from './components/Logo.vue'
import Aws from './components/Aws.vue'

const aws = ref(null)
const running = ref(false)

async function start() {
  running.value = true
  await aws.value.start()
  running.value = false
}

</script>

<template>
  <article class="grid-container min-h-screen flex flex-col">
    <main class="grow py-10">
      <div class="text-center pb-10">
        <h1 class="text-2xl font-semibold mt-2">
          <Logo class="h-12 inline-block"/>
        </h1>
        <p class="mx-auto max-w-prose my-6">
          Utilize this page as a powerful tool to gauge latency, assessing the connection speed from
          your browser to
          diverse datacenters hosted by various cloud providers.
        </p>
        <button
          @click="start"
          :disabled="running"
          class="font-semibold uppercase text-sm bg-pink-600 text-white px-4 py-2 rounded-md hover:bg-pink-700 disabled:opacity-50 disabled:cursor-not-allowed"
        >Start
        </button>
      </div>
      <Aws ref="aws"/>
    </main>
    <div class="py-6 text-center text-sm text-zinc-500"> © 2024 ⋅ A
      <a
        class="text-pink-600 transition-colors hover:text-pink-700"
        href="https://www.coderscantina.com"
        target="_blank"
      >Coder's Cantina</a>
      project ⋅ Built in public ⋅ All rights reserved.
    </div>
  </article>
</template>

<style>
</style>
