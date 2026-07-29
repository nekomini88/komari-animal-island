<script setup lang="ts">
import { computed } from 'vue'
import { useAppStore } from '@/stores/app'

const appStore = useAppStore()
const isDark = computed(() => appStore.isDark)
</script>

<template>
  <div class="loading-cover fixed inset-0 z-20 flex items-center justify-center backdrop-blur-sm">
    <div class="flex flex-col items-center gap-4 text-foreground">
      <div class="loading-leaf" />
      <p class="text-sm font-bold tracking-wide" style="color: var(--foreground)">Loading...</p>
    </div>
  </div>
</template>
