<script setup>
import config from '../config/aws.json'
import RegionItem from './RegionItem.vue'
import { defineExpose, ref } from 'vue'

const itemRefs = ref([])

async function start() {
  await Promise.all(itemRefs.value.map(async (item) => item.ping()))
}

defineExpose({ start })

</script>

<template>
  <section class="">
    <h2 class="text-xl mb-2 font-semibold">{{ config.title }}</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-6">
      <div v-for="item in config.items">
        <RegionItem
          :item="item"
          :url-scheme="config.urlScheme"
          ref="itemRefs"
        />
      </div>
    </div>
  </section>
</template>

<style scoped>
</style>
