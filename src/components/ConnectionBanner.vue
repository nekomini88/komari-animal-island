<script setup lang="ts">
import { Icon } from '@iconify/vue'
import { computed } from 'vue'
import { useAppStore } from '@/stores/app'
import { useNodesStore } from '@/stores/nodes'

const appStore = useAppStore()
const nodesStore = useNodesStore()

const banner = computed(() => {
  if (appStore.loading)
    return null
  if (appStore.connectionError) {
    return {
      tone: 'error' as const,
      icon: 'tabler:plug-connected-x',
      text: '连接服务器失败，数据已停止更新',
    }
  }
  if (nodesStore.wsConnectionState === 'reconnecting') {
    return {
      tone: 'warn' as const,
      icon: 'tabler:refresh',
      text: `实时连接已断开，正在重连（第 ${nodesStore.wsReconnectAttempts} 次）`,
    }
  }
  return null
})
</script>

<template>
  <Transition
    enter-active-class="transition-all duration-200 ease-out"
    enter-from-class="opacity-0 -translate-y-2"
    enter-to-class="opacity-100 translate-y-0"
    leave-active-class="transition-all duration-150 ease-in"
    leave-from-class="opacity-100 translate-y-0"
    leave-to-class="opacity-0 -translate-y-2"
  >
    <div v-if="banner" class="fixed top-3.5 inset-x-0 z-50 flex justify-center pointer-events-none">
      <div
        class="pointer-events-auto flex items-center gap-1.5 h-7 px-3 rounded-full text-xs backdrop-blur-xl shadow-sm ring-1 whitespace-nowrap"
        :class="banner.tone==='error'?'bg-error/15 text-error ring-error/20':'bg-warning/15 text-awesome ring-warning/20'"
        role="status"
      >
        <Icon :icon="banner.icon" width="13" height="13" :class="banner.tone==='warn' && 'animate-spin'" />
        {{ banner.text }}
      </div>
    </div>
  </Transition>
</template>
