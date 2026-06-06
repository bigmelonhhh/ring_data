<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { openSuccessToast, openToast } from '@/components/toast/toast'

const router = useRouter()

// 反馈内容
const feedbackContent = ref('')

/**
 * 返回上一页。
 *
 * @returns {void} 无返回值。
 */
const onClickLeft = () => {
  router.back()
}

/**
 * 提交用户反馈内容。
 *
 * @returns {void} 无返回值。
 */
const handleSubmit = () => {
  if (!feedbackContent.value.trim()) {
    openToast('请输入反馈内容')
    return
  }

  openSuccessToast({
    message: '感谢您的反馈！',
    onClose: () => {
      router.back()
    },
  })
}
</script>

<template>
  <div class="care-page flex min-h-screen flex-col">
    <van-nav-bar
      title="举报与反馈"
      left-arrow
      :border="false"
      class="transparent-nav"
      @click-left="onClickLeft"
    />

    <div class="mt-4 flex-1 px-4 pb-6">
      <div class="care-card mb-4 p-4">
        <p class="text-[13px] font-semibold text-[#00A68E]">产品反馈</p>
        <h1 class="mt-1 text-[22px] font-bold text-[#102A31]">告诉我们你的使用感受</h1>
        <p class="mt-2 text-sm text-[#6B7B86]">无论是问题反馈还是体验建议，我们都会认真记录并持续优化。</p>
      </div>

      <div class="care-card p-1 mb-6">
        <van-field
          v-model="feedbackContent"
          rows="6"
          autosize
          type="textarea"
          maxlength="200"
          placeholder="请输入您的宝贵意见或反馈问题，我们将尽快处理（最多200字）"
          show-word-limit
          class="!bg-transparent"
        />
      </div>

      <van-button
        block
        round
        type="primary"
        class="care-primary-btn  !h-12"
        @click="handleSubmit"
      >
        提交反馈
      </van-button>
    </div>
  </div>
</template>
