<script setup lang="ts">
import { computed } from 'vue'
import dayjs from 'dayjs'
import botImage from '@/assets/bot.webp'
import type { IHealthData } from '@/types/home'

defineProps<{
  healthData: IHealthData
}>()

/**
 * 根据当前时间获取问候语。
 * @param 无入参。
 * @returns 对应的问候语。
 */
const greeting = computed(() => {
  const hour = dayjs().hour()
  if (hour >= 6 && hour < 12) {
    return '上午好~工作久了，起来活动一下吧☕'
  } else if (hour >= 12 && hour < 18) {
    return '下午好~工作久了，起来活动一下吧☕'
  } else {
    return '晚上好~忙碌一天，注意休息哦🌙'
  }
})
</script>

<template>
  <div class="px-4 py-3">
    <div class="health-card relative overflow-hidden rounded-[26px]">
      <!-- 骨架屏 -->
      <div v-if="healthData.isLoading" class="animate-pulse space-y-4 p-5">
        <div class="h-6 w-1/2 rounded-full bg-[#c8ded9]"></div>
        <div class="h-4 w-1/3 rounded-full bg-[#dcebe8]"></div>
        <div class="grid grid-cols-3 gap-2 pt-2">
          <div class="h-16 rounded-[18px] bg-[#dcebe8]"></div>
          <div class="h-16 rounded-[18px] bg-[#dcebe8]"></div>
          <div class="h-16 rounded-[18px] bg-[#dcebe8]"></div>
        </div>
      </div>

      <!-- 真实数据 -->
      <template v-else>
        <div class="relative z-10 px-5 pt-2">
          <div class="flex items-start justify-between gap-2">
            <div>
              <h2 class="mt-3 text-[22px] font-bold leading-tight text-[#102A31]">{{ greeting.split('~')[0] }}~</h2>
              <p class="mt-1 text-[16px] font-semibold leading-snug text-[#334D57]">{{ greeting.split('~')[1] }}</p>
            </div>
            <div class="bot-shell">
              <img class="h-[92px] w-[88px] object-contain" :src="botImage" alt="健康机器人" />
            </div>
          </div>
        </div>

        <div class="relative z-10 space-y-3 px-5 pb-5 ">
          <div class="flex items-center justify-between">
            <h3 class="text-[15px] font-bold text-[#102A31]">健康小提醒</h3>
          </div>

          <template v-if="healthData.hasData">
            <!-- PRD 要求的固定格式文案与异常高亮 -->
            <p class="text-[15px] leading-[24px] text-[#334D57] whitespace-pre-line text-justify">
              您的体温与血氧指标正常，但当前心率偏高（<span class="font-semibold text-[#E77446]">89次/分</span>），压力指数达<span class="font-semibold text-[#E77446]">7级</span>，且睡眠质量78分，说明身体正处于紧绷且休息不足的状态。
            </p>
            <p class="advice-box mt-2 text-[15px] leading-[24px] text-[#213942] whitespace-pre-line text-justify">
              {{ healthData.healthAdvice }}
            </p>
          </template>
          <template v-else>
            <p class="advice-box text-[15px] leading-[24px] text-[#334D57]">暂无健康数据，建议绑定智能设备获取个性化提醒</p>
          </template>
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped>
.health-card {
  min-height: 286px;
  border: 1px solid rgba(255, 255, 255, 0.78);
  background:
    radial-gradient(circle at 88% 12%, rgba(255, 180, 120, 0.28), transparent 28%),
    linear-gradient(135deg, rgba(237, 253, 248, 0.98) 0%, rgba(230, 245, 255, 0.96) 100%);
  box-shadow: 0 20px 46px rgba(16, 42, 49, 0.1);
}

.health-card::before {
  position: absolute;
  inset: auto -24px -42px 46%;
  height: 150px;
  border-radius: 999px;
  background: rgba(0, 166, 142, 0.12);
  content: '';
}

.ai-tag,
.device-pill {
  display: inline-flex;
  align-items: center;
  border-radius: 999px;
  font-size: 12px;
  font-weight: 700;
}

.ai-tag {
  padding: 5px 9px;
  background: rgba(0, 166, 142, 0.12);
  color: #008979;
}

.device-pill {
  padding: 4px 8px;
  background: rgba(46, 132, 255, 0.1);
  color: #2e66c7;
}

.bot-shell {
  display: flex;
  width: 86px;
  height: 86px;
  flex-shrink: 0;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  border-radius: 28px;
}

.metrics-strip {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 8px;
}

.metrics-strip div {
  min-height: 78px;
  padding: 10px 8px;
  border: 1px solid rgba(255, 255, 255, 0.78);
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.66);
}

.metrics-strip span,
.metrics-strip em {
  display: block;
  color: #6b7b86;
  font-size: 11px;
  font-style: normal;
  font-weight: 600;
}

.metrics-strip strong {
  display: inline-block;
  margin-top: 7px;
  color: #102a31;
  font-size: 24px;
  line-height: 1;
}

.advice-box {
  border-radius: 18px;
  background: rgba(255, 255, 255, 0.58);
  padding: 12px;
}
</style>
