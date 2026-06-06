<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import szrBg from '@/assets/szr_male_bg.webp'
import headTx from '@/assets/head_tx.png'
import szrBack from '@/assets/szr_back_bg.webp'
import { openSuccessToast, openToast } from '@/components/toast/toast'
import { getStorage, setStorage } from '@/utils/storage'

const router = useRouter()
const RECORD_NAME_STORAGE_KEY = 'vue3-h5-record-name'
const displayName = ref(getStorage<string>(RECORD_NAME_STORAGE_KEY, '主*'))
const editNamePopupVisible = ref(false)
const draftName = ref(displayName.value)

/**
 * 返回上一页。
 *
 * @returns {void} 无返回值。
 */
const onClickLeft = () => {
  router.back()
}

/**
 * 打开姓名编辑弹窗。
 *
 * @returns {void} 无返回值。
 */
const handleEditName = () => {
  draftName.value = displayName.value
  editNamePopupVisible.value = true
}

/**
 * 关闭姓名编辑弹窗。
 *
 * @returns {void} 无返回值。
 */
const handleCloseEditPopup = () => {
  editNamePopupVisible.value = false
}

/**
 * 保存用户输入的档案名称。
 *
 * @returns {void} 无返回值。
 */
const handleSaveName = () => {
  const nextName = draftName.value.trim()

  if (!nextName) {
    openToast('请输入姓名')
    return
  }

  displayName.value = nextName
  setStorage(RECORD_NAME_STORAGE_KEY, nextName)
  editNamePopupVisible.value = false
  openSuccessToast({
    message: '保存成功',
  })
}

/**
 * 处理功能菜单点击事件。
 *
 * @param {string} name 当前点击的菜单名称。
 * @returns {void} 无返回值。
 */
const handleMenuClick = (name: string) => {
  openToast(`点击了：${name}`)
}

// 功能菜单数据
const menuList = [
  { name: '智能设备' },
  { name: '提醒设置' },
  { name: '亲情账号' },
  { name: '意见反馈' },
]
</script>

<template>
  <div class="care-page relative flex min-h-screen flex-col overflow-hidden">
    <van-nav-bar
      title="健康档案"
      left-arrow
      :border="false"
      class="transparent-nav relative z-10"
      @click-left="onClickLeft"
    >
    </van-nav-bar>

    <div class="relative z-10 w-full px-4 pt-2 pb-4">
      <div class="absolute left-4 top-2 flex items-center rounded-r-full rounded-tl-full bg-gradient-to-r from-[#00A68E] to-[#2E84FF] p-2 pr-3 shadow-md">
        <van-image
          round
          width="24"
          height="24"
          :src="headTx"
          class="border border-white"
        />
        <span class="text-white text-xs ml-2 font-medium">本人</span>
      </div>

      <div class="flex items-center justify-center mt-8">
        <span class="text-xl font-bold tracking-wider text-[#102A31]">{{ displayName }}</span>
        <van-icon name="edit" class="ml-2 text-[#6B7B86]" size="18" @click="handleEditName" />
      </div>
    </div>

    <div class="relative w-full flex items-end justify-center pt-10">
      <div class="relative w-full h-[240px] flex justify-center items-center">
        <img
          :src="szrBg"
          alt="数字人背景"
          class="absolute inset-0 z-20 h-full w-full object-contain"
        />
        <img
          :src="szrBack"
          alt="数字人"
          class="relative z-10 max-h-[240px] object-contain"
        />
      </div>

      <div class="absolute left-8 top-[30%] flex items-center group z-20">
        <div class="flex flex-col items-start mr-2">
          <span class="text-xl font-bold text-[#102A31]">男</span>
          <span class="text-xs text-[#6B7B86]">性别</span>
        </div>
        <div class="relative w-12 h-[1px] bg-[#96C5F7]">
          <div class="absolute left-0 top-1/2 -translate-y-1/2 w-1 h-5 bg-[#1890FF] rounded-full"></div>
          <div class="absolute right-0 top-1/2 -translate-y-1/2 w-1.5 h-1.5 bg-[#1890FF] rounded-full border border-white"></div>
        </div>
      </div>

      <div class="absolute left-8 top-[65%] flex items-center group z-20">
        <div class="flex flex-col items-start mr-2">
          <span class="text-xl font-bold text-[#102A31]">38岁</span>
          <span class="text-xs text-[#6B7B86]">年龄</span>
        </div>
        <div class="relative w-10 h-[1px] bg-[#96C5F7]">
          <div class="absolute left-0 top-1/2 -translate-y-1/2 w-1 h-5 bg-[#1890FF] rounded-full"></div>
          <div class="absolute right-0 top-1/2 -translate-y-1/2 w-1.5 h-1.5 bg-[#1890FF] rounded-full border border-white"></div>
        </div>
      </div>

      <div class="absolute right-8 top-[35%] flex items-center group z-20">
        <div class="relative w-12 h-[1px] bg-[#96C5F7] mr-2">
          <div class="absolute right-0 top-1/2 -translate-y-1/2 w-1 h-5 bg-[#1890FF] rounded-full"></div>
          <div class="absolute left-0 top-1/2 -translate-y-1/2 w-1.5 h-1.5 bg-[#1890FF] rounded-full border border-white"></div>
        </div>
        <div class="flex flex-col items-end">
          <span class="text-xl font-bold text-[#102A31]">22.9</span>
          <div class="flex items-center gap-1">
            <span class="text-xs text-[#6B7B86]">BMI</span>
            <van-icon name="info-o" size="12" color="#6B7B86" />
          </div>
          <div class="mt-1 bg-gradient-to-r from-[#4ADE80] to-[#52C41A] text-white text-[10px] px-2 py-0.5 rounded shadow-sm">正常</div>
        </div>
      </div>
    </div>

    <div class="z-30 mt-4 w-full flex-1 rounded-t-[32px] border border-white/60 bg-white/35 px-4 pt-8 shadow-[0_-10px_30px_rgba(16,42,49,0.08)] backdrop-blur-xl">
      <div class="care-card grid grid-cols-4 gap-4 p-4">
        <div 
          v-for="item in menuList"
          :key="item.name"
          class="flex flex-col items-center gap-2 cursor-pointer active:opacity-70 transition-opacity"
          @click="handleMenuClick(item.name)"
        >
          <div class="flex h-14 w-14 items-center justify-center rounded-2xl bg-[#E7F6F2] shadow-inner">
          </div>
          <span class="text-xs text-[#4E5969]">{{ item.name }}</span>
        </div>
      </div>
    </div>

    <van-popup
      v-model:show="editNamePopupVisible"
      round
      close-on-click-overlay
      class="name-edit-popup w-[320px]"
      @click-overlay="handleCloseEditPopup"
    >
      <div class="px-5 pb-5 pt-6 w-[300px]">
        <div class="text-center text-lg font-semibold text-[#102A31]">编辑姓名</div>
        <div class="mt-4 overflow-hidden rounded-2xl bg-[#F4FAF7]">
          <van-field
            v-model="draftName"
            maxlength="12"
            placeholder="请输入姓名"
            clearable
            input-align="left"
          />
        </div>
        <div class="mt-5 flex gap-3">
          <van-button plain block round class="!h-11 !border-[#D9DEE8] !text-[#4E5969]" @click="handleCloseEditPopup">
            取消
          </van-button>
          <van-button block round type="primary" class="care-primary-btn !h-11" @click="handleSaveName">
            确定
          </van-button>
        </div>
      </div>
    </van-popup>
  </div>
</template>

<style scoped>
:deep(.van-nav-bar) {
  background-color: transparent;
}
:deep(.van-nav-bar__title) {
  color: #1D2129;
  font-weight: bold;
}
:deep(.van-nav-bar .van-icon) {
  color: #1D2129;
}

.name-edit-popup {
  overflow: hidden;
}

.name-edit-popup :deep(.van-cell) {
  background: transparent;
}

.name-edit-popup :deep(.van-field__control) {
  color: #102a31;
}

.name-edit-popup :deep(.van-cell::after) {
  display: none;
}
</style>
