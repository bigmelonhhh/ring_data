<script setup lang="ts">
import { storeToRefs } from 'pinia'
import { ref, onMounted, onUnmounted, nextTick, watch } from 'vue'
import HomeTopBar from './components/HomeTopBar.vue'
import HealthAlertCard from './components/HealthAlertCard.vue'
import QuickActions from './components/QuickActions.vue'
import ChatBubble from './components/ChatBubble.vue'
import ChatInput from './components/ChatInput.vue'
import ComplianceFooter from './components/ComplianceFooter.vue'
import { useChatStore } from '@/stores/chat'
import { useHomeStore } from '@/stores/home'

const chatStore = useChatStore()
const homeStore = useHomeStore()
homeStore.hydrateFromStorage()

const { activeMessages } = storeToRefs(chatStore)
const { healthData } = storeToRefs(homeStore)
const scrollContainer = ref<HTMLElement | null>(null)
let pollTimer: ReturnType<typeof setInterval> | null = null

/**
 * 启动健康数据轮询。
 * @param 无入参。
 * @returns 无返回结果。
 */
function startPolling(): void {
  pollTimer = setInterval(() => {
    homeStore.refreshHealthData()
  }, 3 * 60 * 1000)
}

/**
 * 处理发送消息。
 * @param text 用户输入文本。
 * @returns 无返回结果。
 */
function handleSend(text: string): void {
  chatStore.sendMessage(text)
}

/**
 * 滚动到最底部。
 * @param 无入参。
 * @returns 无返回结果。
 */
function scrollToBottom(): void {
  nextTick(() => {
    if (scrollContainer.value) {
      scrollContainer.value.scrollTop = scrollContainer.value.scrollHeight
    }
  })
}

onMounted(() => {
  chatStore.hydrateFromStorage()
  homeStore.ensureHealthData()
  startPolling()
})

watch(
  activeMessages,
  () => {
    scrollToBottom()
  },
  {
    deep: true,
    flush: 'post',
  },
)

onUnmounted(() => {
  if (pollTimer) {
    clearInterval(pollTimer)
  }
})
</script>

<template>
  <div class="home-page flex h-screen flex-col">
    <div class="shrink-0">
      <HomeTopBar />
    </div>

    <main ref="scrollContainer" class="flex-1 min-h-0 overflow-y-auto pt-2 custom-scrollbar pb-4">
      <HealthAlertCard :health-data="healthData" />

      <div class="mt-2 space-y-4">
        <ChatBubble v-for="msg in activeMessages" :key="msg.id" :msg="msg" />
      </div>
    </main>

    <footer class="fixed-footer flex-shrink-0">
      <QuickActions />
      <ChatInput @send="handleSend" />
      <ComplianceFooter />
    </footer>
  </div>
</template>

<style scoped>
.home-page {
  background:
    radial-gradient(circle at 12% 4%, rgba(111, 226, 207, 0.22), transparent 30%),
    radial-gradient(circle at 86% 18%, rgba(255, 196, 142, 0.18), transparent 25%),
    linear-gradient(180deg, #f7fffc 0%, #edf8f4 52%, #f8fbff 100%);
}

.custom-scrollbar::-webkit-scrollbar {
  display: none;
}
.custom-scrollbar {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.fixed-footer {
  padding-bottom: calc(env(safe-area-inset-bottom) + 6px);
  background: rgba(247, 252, 250, 0.9);
  box-shadow: 0 -12px 34px rgba(18, 34, 46, 0.08);
  backdrop-filter: blur(16px);
}
</style>
