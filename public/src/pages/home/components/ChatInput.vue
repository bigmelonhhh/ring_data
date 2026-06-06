<script setup lang="ts">
import { ref, watch } from 'vue'
import { openToast } from '@/components/toast/toast'

const emit = defineEmits<{
  (e: 'send', text: string): void
}>()

const inputText = ref('')
const isOverLimit = ref(false)

// 监控字数
watch(inputText, (val) => {
  if (val.length >= 500) {
    isOverLimit.value = true
  } else {
    isOverLimit.value = false
  }
})

/**
 * 拦截输入事件，超过 500 字提示并阻止输入。
 * @param val 当前输入值。
 * @returns 无返回结果。
 */
function handleInput(val: string): void {
  if (val.length > 500) {
    inputText.value = val.slice(0, 500)
    openToast({
      message: '输入内容不能超过500字',
      position: 'bottom',
    })
  }
}

/**
 * 处理发送逻辑。
 * @param 无入参。
 * @returns 无返回结果。
 */
function handleSend(): void {
  const text = inputText.value.trim()
  if (!text) {
    // PRD: 输入内容为空，不触发请求，可选震动
    if (navigator.vibrate) {
      navigator.vibrate(100)
    }
    return
  }
  emit('send', text)
  inputText.value = ''
}
</script>

<template>
  <div class="px-4">
    <!-- 超出字数提示 -->
    <div v-if="isOverLimit" class="mb-2 text-center text-[14px] text-[#E77446]">
      输入内容不能超过500字
    </div>

    <div class="chat-input-box flex items-center rounded-[24px] px-4 py-[10px] min-h-[50px]">
      <van-field
        v-model="inputText"
        class="!p-0 !bg-transparent flex-1"
        type="textarea"
        autosize
        rows="1"
        placeholder="有什么健康问题问我吗"
        @update:model-value="handleInput"
      />
      <div
        class="ml-3 flex h-9 w-9 items-center justify-center rounded-full transition-colors"
        :class="inputText.trim().length ? 'send-active text-white cursor-pointer' : 'bg-[#DDE8E5] text-[#94A3A0] cursor-not-allowed'"
        @click="handleSend"
      >
        <van-icon name="guide-o" size="16" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.chat-input-box {
  border: 1px solid rgba(255, 255, 255, 0.76); /* 默认边框 */
  background: rgba(255, 255, 255, 0.86);
  box-shadow: 0 12px 30px rgba(16, 42, 49, 0.08);
  transition: border-color 0.2s ease;
}

.chat-input-box:focus-within {
  border-color: rgba(0, 166, 142, 0.42); /* 聚焦时边框 */
}

.send-active {
  background: linear-gradient(135deg, #00a68e, #2e84ff);
  box-shadow: 0 8px 18px rgba(0, 166, 142, 0.2);
}

/* 穿透修改 vant 样式 */
:deep(.van-cell::after) {
  border-bottom: none;
}

:deep(.van-field__control) {
  color: #12222e;
  font-size: 16px;
  line-height: 22px;
}

:deep(.van-field__control::placeholder) {
  color: #8a9aa6;
}
</style>
