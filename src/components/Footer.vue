<script setup lang="ts">
import type { VersionInfo } from '@/utils/api'
import { computed, onMounted, ref } from 'vue'
import { useAppStore } from '@/stores/app'
import { getSharedApi } from '@/utils/api'

const appStore = useAppStore()
const api = getSharedApi()

const buildVersion = __BUILD_VERSION__
const buildGitHash = __BUILD_GIT_HASH__

const serverVersion = ref<VersionInfo | null>(null)

onMounted(async () => {
  try {
    serverVersion.value = await api.getVersion()
  }
  catch {
    // 静默失败
  }
})

const formattedServerVersion = computed(() => serverVersion.value?.version ?? '')

const showIcp = computed(() => appStore.icpEnabled && appStore.icpNumber)
const showPolice = computed(() => appStore.policeEnabled && appStore.policeNumber)
const showFiling = computed(() => showIcp.value || showPolice.value)
</script>

<template>
  <footer class="w-full max-w-[1280px] mx-auto px-4 pt-2 pb-4">
    <div class="ac-footer-wave w-full" />
    <div class="flex flex-col sm:flex-row gap-2 sm:items-center sm:justify-between text-xs" style="color: var(--foreground)">
      <div class="flex flex-row flex-wrap items-center gap-x-3 gap-y-1">
        <span class="inline-flex items-center gap-1 font-bold">
          <img src="/src/assets/img/icons/icon-leaf.png" class="size-4" alt="leaf" />
          Powered by Komari Monitor
        </span>
        <span class="opacity-40">·</span>
        <span>Theme by <strong>Komari Animal Island</strong></span>
      </div>
      <div class="flex flex-wrap gap-2 items-center">
        <span class="opacity-60">v{{ buildVersion }}</span>
        <span v-if="showFiling" class="opacity-60">·</span>
        <template v-if="showIcp">
          <a v-if="appStore.icpUrl" :href="appStore.icpUrl" target="_blank" rel="noopener noreferrer" class="hover:opacity-80">
            <span>{{ appStore.icpNumber || '' }}</span>
          </a>
          <span v-else class="opacity-60">{{ appStore.icpNumber || '' }}</span>
        </template>
        <template v-if="showPolice">
          <a v-if="appStore.policeUrl" :href="appStore.policeUrl" target="_blank" rel="noopener noreferrer" class="hover:opacity-80">
            <span>{{ appStore.policeNumber || '' }}</span>
          </a>
          <span v-else class="opacity-60">{{ appStore.policeNumber || '' }}</span>
        </template>
      </div>
    </div>
  </footer>
</template>
