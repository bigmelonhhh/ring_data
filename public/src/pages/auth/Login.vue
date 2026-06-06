<script setup lang="ts">
import type { FormInstance } from 'vant'
import { reactive, ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import logoMark from '@/assets/logo.webp'
import { openSuccessToast } from '@/components/toast/toast'
import { useUserStore } from '@/stores/user'
import type { ILoginForm } from '@/types/user'
import { isMobile, isPassword } from '@/utils/validate'

const route = useRoute()
const router = useRouter()
const userStore = useUserStore()
const formRef = ref<FormInstance>()
const form = reactive<ILoginForm>({
  mobile: '13800138000',
  password: 'abc12345',
})

// 控制密码显示隐藏
const showPassword = ref(false)

/**
 * 切换密码显示隐藏状态。
 * @param 无入参。
 * @returns 无返回结果。
 */
function togglePasswordVisibility(): void {
  showPassword.value = !showPassword.value
}

/**
 * 获取登录成功后的跳转地址。
 * @param 无入参。
 * @returns 目标路由地址。
 */
function getRedirectPath(): string {
  return String(route.query.redirect || '/home')
}

/**
 * 提交登录表单。
 * @param 无入参。
 * @returns 无返回结果。
 */
async function handleSubmit(): Promise<void> {
  await formRef.value?.validate()
  userStore.loginByDemo(form)
  openSuccessToast('登录成功，欢迎回来')
  router.replace(getRedirectPath())
}
</script>

<template>
  <div class="login-page min-h-screen px-5 pb-6 pt-10">
    <section class="login-hero">
      <div class="brand-row">
        <div class="brand-mark">
          <img class="h-9 object-cover" :src="logoMark" alt="logo" />
        </div>
        <div>
          <h1 class="mt-1 text-[20px] font-bold leading-tight text-[#102A31]">你的健康数据助手</h1>
        </div>
      </div>
    </section>

    <section class="login-panel rounded-[28px] px-5 py-6">
      <div class="mb-6">
        <p class="text-[18px] font-semibold text-[#00A68E]">欢迎回来</p>
      </div>

      <van-form ref="formRef" class="space-y-6" @submit="handleSubmit">
        <div class="space-y-4">
          <div class="neu-input rounded-[20px] overflow-hidden">
            <van-field
              v-model="form.mobile"
              name="mobile"
              label="手机号"
              placeholder="请输入手机号"
              :rules="[
                { required: true, message: '请输入手机号' },
                { validator: isMobile, message: '请输入正确的手机号' },
              ]"
            />
          </div>

          <div class="neu-input rounded-[20px] overflow-hidden">
            <van-field
              v-model="form.password"
              name="password"
              :type="showPassword ? 'text' : 'password'"
              label="密码"
              placeholder="请输入密码"
              :right-icon="showPassword ? 'eye-o' : 'closed-eye'"
              :rules="[
                { required: true, message: '请输入密码' },
                { validator: isPassword, message: '密码需为 6-20 位字母数字组合' },
              ]"
              @click-right-icon="togglePasswordVisibility"
            />
          </div>
        </div>

        <div class="pt-2 space-y-4">
          <button type="submit" class="neu-btn w-full h-12 rounded-full font-semibold text-[17px] flex items-center justify-center">
            登录
          </button>
        </div>
      </van-form>
    </section>
  </div>
</template>

<style scoped>
.login-page {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  background:
    radial-gradient(circle at 82% 10%, rgba(255, 204, 153, 0.34), transparent 26%),
    radial-gradient(circle at 12% 18%, rgba(106, 224, 205, 0.32), transparent 30%),
    linear-gradient(180deg, #f7fffc 0%, #edf8f4 54%, #f9fbff 100%);
}

.login-hero {
  padding-top: 22px;
}

.brand-row {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 14px;
}

.brand-mark {
  display: flex;
  height: 60px;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(255, 255, 255, 0.72);
  border-radius: 22px;
  box-shadow: 0 16px 36px rgba(0, 105, 92, 0.1);
  backdrop-filter: blur(14px);
}

.login-panel {
  width: 100%;
  margin-top: clamp(88px, 16vh, 100px);
  background: rgba(255, 255, 255, 0.86);
  box-shadow: 0 22px 56px rgba(16, 42, 49, 0.12);
  backdrop-filter: blur(18px);
}

.neu-input {
  background: rgba(247, 251, 250, 0.92);
  box-shadow: inset 0 0 0 1px rgba(0, 166, 142, 0.12);
}

.neu-btn {
  background: linear-gradient(135deg, #00a68e 0%, #2e84ff 100%);
  box-shadow: 0 14px 26px rgba(0, 166, 142, 0.24);
  color: white;
  transition: all 0.2s ease-in-out;
  border: none;
  cursor: pointer;
}

.neu-btn:active {
  transform: translateY(1px);
  box-shadow: inset 0 4px 10px rgba(9, 46, 55, 0.22);
}

.safe-dot {
  display: inline-flex;
  width: 7px;
  height: 7px;
  border-radius: 999px;
  background: #00a68e;
  box-shadow: 0 0 0 4px rgba(0, 166, 142, 0.12);
}

/* 穿透修改 vant 样式 */
:deep(.van-cell) {
  background: transparent !important;
  padding: 16px;
}
:deep(.van-cell::after) {
  display: none;
}
:deep(.van-field__control) {
  color: #12222e;
  font-weight: 500;
}
:deep(.van-field__control::placeholder) {
  color: #8a9aa6;
}
:deep(.van-field__label) {
  color: #12222e;
  font-weight: 600;
}
:deep(.van-field__right-icon) {
  color: #00a68e;
}
</style>
