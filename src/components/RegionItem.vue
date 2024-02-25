<script setup>

import { computed, defineExpose, defineProps, ref } from 'vue'

const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
  urlScheme: {
    type: String,
    required: true,
  },
})

const duration = ref(undefined)

function currentTimeMillis() {
  return (new Date()).getTime();
}


async function ping() {
  const url = props.urlScheme.replace(/{region}/ig, props.item.region)

  const start = performance.now()
  await fetch(`${url}`, {
    mode: 'cors',
    cache: "no-store",
    credentials: "omit",
    redirect: "error",
  })
  duration.value = performance.now() - start
}

const formattedDuration = computed(() => {
  if (duration.value === undefined) {
    return '–'
  }
  const result = Math.round(duration.value * 10) / 10

  return `${result} ms`
})

const durationColor = computed(() => {
  if (duration.value === undefined) {
    return 'text-zinc-400 dark:text-zinc-500'
  }
  if (duration.value < 50) {
    return 'text-green-600 dark:text-green-500'
  }
  if (duration.value < 100) {
    return 'text-yellow-600 dark:text-yellow-500'
  }

  return 'text-red-600 dark:text-red-500'
})

const statusColor = computed(() => {
  if (duration.value === undefined) {
    return 'bg-zinc-400 dark:bg-zinc-600'
  }
  if (duration.value < 50) {
    return 'bg-green-500'
  }
  if (duration.value < 100) {
    return 'bg-yellow-500'
  }

  return 'bg-red-500'
})

defineExpose({
  ping,
})

</script>

<template>
  <div
    class="px-4 py-1 rounded-lg flex items-center bg-zinc-800 cursor-pointer"
    @click.prevent="ping"
  >
    <span :class="['h-3 w-3 rounded-full mr-4', statusColor]"></span>
    <div class="grow">
      <div class="flex items-center">
        <h4 class="font-semibold mr-2">{{ item.name }}</h4>
      </div>
      <div :class="durationColor">{{ formattedDuration }}</div>
    </div>
    <div class="ml-auto text-zinc-500 text-xs sm:text-sm">{{ item.region }}</div>
  </div>
</template>

<style scoped>
</style>

