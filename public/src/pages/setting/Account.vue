<script setup lang="ts">
import { useRouter } from 'vue-router'
import { openDangerConfirmDialog } from '@/components/dialog/dialog'
import { openToast } from '@/components/toast/toast'

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
 * 模拟修改密码操作。
 *
 * @returns {void} 无返回值。
 */
const handleChangePassword = () => {
  openToast('跳转到修改密码页面')
}

/**
 * 执行账号注销确认流程。
 *
 * @returns {Promise<void>} 返回注销流程的 Promise。
 */
const handleCancelAccount = async () => {
  const confirmed = await openDangerConfirmDialog('风险提示', '账号注销后无法恢复，确定要注销吗？')

  if (!confirmed) {
    return
  }

  openToast('账号已注销')
  router.replace('/login')
}
</script>

<template>
  <div class="care-page flex min-h-screen flex-col">
    <van-nav-bar
      title="账号设置"
      left-arrow
      :border="false"
      class="transparent-nav"
      @click-left="onClickLeft"
    />

    <div class="mt-4 flex-1 px-4 pb-6">
      <div class="care-card mb-4 p-4">
        <p class="text-[13px] font-semibold text-[#00A68E]">账号安全</p>
        <h1 class="mt-1 text-[22px] font-bold text-[#102A31]">维护你的登录与绑定信息</h1>
        <p class="mt-2 text-sm text-[#6B7B86]">支持查看绑定状态、修改密码以及管理风险操作。</p>
      </div>

      <van-cell-group inset class="care-cell-group !mx-0">
        <van-cell title="手机号码" value="138****8888" />
        <van-cell title="微信绑定" value="已绑定" />
        <van-cell title="修改密码" is-link @click="handleChangePassword" />
      </van-cell-group>

      <div class="mt-4">
        <van-cell-group inset class="care-cell-group !mx-0">
          <van-cell is-link @click="handleCancelAccount">
            <template #title>
              <span class="font-semibold text-[#E77446]">注销账号</span>
            </template>
            <template #label>
              <span class="text-[#6B7B86]">注销后账号数据将无法恢复</span>
            </template>
          </van-cell>
        </van-cell-group>
      </div>
    </div>
  </div>
</template>
