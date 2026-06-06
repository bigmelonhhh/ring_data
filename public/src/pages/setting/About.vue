<script setup lang="ts">
import { useRouter } from 'vue-router'
import logoMark from '@/assets/logo.webp'

const router = useRouter()

/**
 * 返回上一页。
 *
 * @returns {void} 无返回值。
 */
const onClickLeft = () => {
  router.back()
}
</script>

<template>
  <div class="care-page flex min-h-screen flex-col">
    <van-nav-bar
      title="关于我们"
      left-arrow
      :border="false"
      class="transparent-nav"
      @click-left="onClickLeft"
    />

    <div class="flex-1 px-4 pb-6 pt-4">
      <div class="care-card flex flex-col items-center p-6">
        <div class="flex  items-center justify-center">
          <img :src="logoMark" alt="logo" class="h-15 w-60  object-fit" />
        </div>
        <p class="mt-4 text-xl font-bold text-[#102A31]">关于智医康</p>
        <p class="mt-2 text-sm text-[#6B7B86]">连接生命，赋能健康</p>
        <p class="mt-2 text-sm text-[#6B7B86]">Version 1.0.0</p>
      </div>

      <div class="care-card mt-4 p-5">
        <p class="text-base leading-7 text-[#4E5969]">
          海智医康科技有限公司是一家专注于慢病数字化疗法的前沿医疗科技企业。我们深度融合 AI 大模型能力与医疗级物联网硬件，通过结构化真实世界数据（RWD）赋能医生决策，提升患者生存质量与生存期。智医康创始团队由来自平安、哈啰、上海三甲医院的人工智能、医学、物联网专家组成，我们致力于帮助慢病患者科学康复，并坚持以循证医学为底座，助力“健康中国2030”建设。
        </p>
      </div>

      <div class="care-card mt-4 p-5 text-center text-xs text-[#6B7B86]">
        <p>© 2026 上海智医康科技有限公司 | 沪ICP备2024092528号-4 | 沪公网安备31010402336532号</p>
        <p class="mt-2">地址：上海市徐汇区龙漕路200弄28号302 | 邮箱：kefu@izencare.com | 电话：4006898982</p>
      </div>
    </div>
  </div>
</template>
