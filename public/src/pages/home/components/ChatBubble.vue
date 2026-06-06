<script setup lang="ts">
import { computed } from 'vue'
import type { IChatMessage } from '@/types/chat'

const props = defineProps<{
  msg: IChatMessage
}>()

const isUser = computed(() => props.msg.role === 'user')
</script>

<template>
  <div class="px-4 flex" :class="isUser ? 'justify-end' : 'justify-start'">
    <div
      class="chat-bubble max-w-[85%] px-4 py-3"
      :class="
        isUser
          ? 'chat-bubble-user rounded-bl-[18px] rounded-br-[18px] rounded-tl-[18px] rounded-tr-[6px] text-white'
          : 'chat-bubble-ai rounded-bl-[6px] rounded-br-[18px] rounded-tr-[18px] rounded-tl-[18px] text-[#12222E]'
      "
    >
      <div v-if="msg.status === 'loading'" class="flex items-center gap-2">
        <van-loading type="spinner" size="16" color="#00A68E" />
        <span class="text-base text-[#6B7B86]">AI正在思考...</span>
      </div>
      <div
        v-else
        class="text-base leading-[24px] whitespace-pre-wrap break-words"
        :class="!isUser && msg.status === 'error' ? 'text-[#E77446]' : ''"
      >
        {{ msg.content }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.chat-bubble {
  box-shadow: 0 10px 26px rgba(16, 42, 49, 0.07);
}

.chat-bubble-user {
  background: linear-gradient(135deg, #00a68e 0%, #2e84ff 100%);
}

.chat-bubble-ai {
  border: 1px solid rgba(255, 255, 255, 0.78);
  background: rgba(255, 255, 255, 0.82);
}

</style>
