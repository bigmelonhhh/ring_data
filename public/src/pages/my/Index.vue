<script setup lang="ts">
import { storeToRefs } from 'pinia'
import { useRouter } from 'vue-router'
import { openConfirmDialog } from '@/components/dialog/dialog'
import { useUserStore } from '@/stores/user'
import { formatMobile } from '@/utils/format'

const router = useRouter()
const userStore = useUserStore()
const { profile } = storeToRefs(userStore)

/**
 * 退出当前账号。
 * @param 无入参。
 * @returns 无返回结果。
 */
async function handleLogout(): Promise<void> {
  const confirmed = await openConfirmDialog('退出登录', '确认退出当前账号吗？')

  if (!confirmed) {
    return
  }

  userStore.logout()
  router.replace('/home')
}
</script>

<template>
  <div class="my-page min-h-screen px-4 pb-8 pt-4">
    <section class="profile-card rounded-[28px] px-5 py-6">
      <div class="flex items-center gap-4">
        <img class="h-16 w-16 rounded-full border-2 border-white/80" :src="profile.avatar" alt="avatar" />
        <div>
          <p class="text-[13px] font-semibold text-[#00A68E]">健康账户</p>
          <h1 class="mt-1 text-xl font-bold text-[#102A31]">{{ profile.name }}</h1>
          <p class="mt-1 text-sm text-[#5E707E]">{{ formatMobile(profile.mobile) }}</p>
        </div>
      </div>

      <div class="mt-5 grid grid-cols-3 gap-3">
        <div class="metric-card px-3 py-3 text-center">
          <p class="text-lg font-bold text-[#102A31]">12</p>
          <p class="mt-1 text-xs text-[#6B7B86]">健康咨询</p>
        </div>
        <div class="metric-card px-3 py-3 text-center">
          <p class="text-lg font-bold text-[#102A31]">3</p>
          <p class="mt-1 text-xs text-[#6B7B86]">绑定设备</p>
        </div>
        <div class="metric-card px-3 py-3 text-center">
          <p class="text-lg font-bold text-[#102A31]">98%</p>
          <p class="mt-1 text-xs text-[#6B7B86]">数据同步</p>
        </div>
      </div>
    </section>

    <section class="section-card mt-4">
      <div class="flex items-center justify-between">
        <h2 class="text-[17px] font-bold text-[#102A31]">健康服务</h2>
        <span class="rounded-full bg-[#E6FAF4] px-3 py-1 text-xs font-semibold text-[#008979]">AI + 硬件</span>
      </div>
      <div class="mt-4 grid grid-cols-2 gap-3">
        <div class="service-card px-4 py-4">
          <p class="text-base font-bold text-[#102A31]">AI 健康咨询</p>
          <p class="mt-2 text-sm leading-[20px] text-[#6B7B86]">围绕症状、指标和用药提供健康建议</p>
        </div>
        <div class="service-card px-4 py-4">
          <p class="text-base font-bold text-[#102A31]">设备数据</p>
          <p class="mt-2 text-sm leading-[20px] text-[#6B7B86]">同步心率、血氧、睡眠等硬件指标</p>
        </div>
      </div>
    </section>

    <van-button block class="logout-btn !mt-5 !h-12 !rounded-full" @click="handleLogout">
      退出登录
    </van-button>
  </div>
</template>

<style scoped>
.my-page {
  background:
    radial-gradient(circle at 12% 4%, rgba(111, 226, 207, 0.22), transparent 30%),
    radial-gradient(circle at 86% 18%, rgba(255, 196, 142, 0.18), transparent 25%),
    linear-gradient(180deg, #f7fffc 0%, #edf8f4 52%, #f8fbff 100%);
}

.profile-card,
.section-card {
  border: 1px solid rgba(255, 255, 255, 0.78);
  background: rgba(255, 255, 255, 0.78);
  box-shadow: 0 18px 42px rgba(16, 42, 49, 0.09);
  backdrop-filter: blur(14px);
}

.metric-card,
.service-card {
  border: 1px solid rgba(255, 255, 255, 0.76);
  border-radius: 20px;
  background: rgba(246, 251, 250, 0.86);
}

.section-card {
  border-radius: 26px;
  padding: 18px;
}

.logout-btn {
  border: none !important;
  background: rgba(255, 255, 255, 0.72) !important;
  color: #e77446 !important;
  font-weight: 700;
  box-shadow: inset 0 0 0 1px rgba(231, 116, 70, 0.18);
}
</style>
