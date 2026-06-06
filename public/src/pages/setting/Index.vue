<script setup lang="ts">
import { useRouter } from 'vue-router'
import { openConfirmDialog } from '@/components/dialog/dialog'

const router = useRouter()

/**
 * 返回上一页。
 *
 * @returns {void} 无返回值。
 */
const onClickLeft = () => {
  router.back()
}

/**
 * 退出当前登录状态。
 *
 * @returns {Promise<void>} 返回退出登录流程的 Promise。
 */
const handleLogout = async () => {
  const confirmed = await openConfirmDialog('提示', '确定要退出登录吗？')

  if (!confirmed) {
    return
  }

  router.replace('/login')
}
</script>

<template>
  <div class="care-page flex flex-col">
    <!-- 顶部导航栏 -->
    <van-nav-bar
      title="系统设置"
      left-arrow
      :border="false"
      class="transparent-nav"
      @click-left="onClickLeft"
    />

    <!-- 设置菜单列表 -->
    <div class="mt-4 flex-1 px-4 pb-6">
      <div class="care-card mb-4 p-4">
        <p class="text-[13px] font-semibold text-[#00A68E]">账户与服务</p>
        <h1 class="mt-1 text-[22px] font-bold text-[#102A31]">管理你的健康数据体验</h1>
        <p class="mt-2 text-sm text-[#6B7B86]">统一管理账号安全、反馈渠道与平台说明。</p>
      </div>

      <van-cell-group inset class="care-cell-group !mx-0">
        <van-cell icon="friends" title="账号设置" is-link to="/setting/account" />
        <van-cell icon="share" title="举报与反馈" is-link to="/setting/feedback" />
        <van-cell icon="info" title="关于我们" is-link to="/setting/about" />
      </van-cell-group>

      <!-- 退出登录按钮 -->
      <div class="mt-8">
        <van-button 
          block 
          round 
          class="care-danger-btn !h-12"
          @click="handleLogout"
        >
          退出登录
        </van-button>
      </div>
    </div>
  </div>
</template>
