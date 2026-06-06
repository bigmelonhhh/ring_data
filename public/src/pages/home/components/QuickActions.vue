<script setup lang="ts">
import { openToast } from '@/components/toast/toast'

const actions = [
  { id: 'consult', title: '健康咨询', desc: '真人医生在线解答', route: '/consult' },
  { id: 'medicine', title: '用药提醒', desc: '系统级提醒你吃药', route: '/medicine' },
  { id: 'device-change', title: '设备更换', desc: '退换/管理', route: '/device-change' },
]

/**
 * 处理快捷按钮点击事件。
 * @param routePath 目标路由。
 * @returns 无返回结果。
 */
function handleAction(_routePath: string): void {
  // 因为当前项目只保留了 /home, /my, /login，其他页面不存在
  // 根据 PRD：“若跳转失败，页面顶部展示「跳转失败，请重试」提示”
  openToast({
    message: '跳转失败，请重试',
    position: 'top',
    className: 'custom-toast-fail',
  })
}
</script>

<template>
  <div class="px-4 pb-2 pt-2">
    <div class="flex gap-3 overflow-x-auto pb-2 scrollbar-hide">
      <div
        v-for="action in actions"
        :key="action.id"
        class="quick-card min-w-[148px] flex-shrink-0 cursor-pointer rounded-[20px] px-[14px] py-[11px]"
        @click="handleAction(action.route)"
      >
        <div class="flex items-center gap-2">
          <span class="quick-dot"></span>
          <div class="text-[15px] font-bold text-[#102A31]">{{ action.title }}</div>
        </div>
        <div class="mt-1 text-[12px] font-medium text-[#6B7B86] whitespace-nowrap">{{ action.desc }}</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.scrollbar-hide::-webkit-scrollbar {
  display: none;
}
.scrollbar-hide {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.quick-card {
  border: 1px solid rgba(227, 239, 234, 0.96);
  background: rgba(231, 243, 238, 0.96);
  box-shadow: 0 2px 8px rgba(16, 42, 49, 0.03);
  transition:
    background-color 0.15s ease-in-out,
    border-color 0.15s ease-in-out,
    box-shadow 0.15s ease-in-out,
    transform 0.15s ease-in-out;
}

.quick-card:active {
  background: rgba(221, 237, 230, 0.98);
  transform: translateY(1px);
}

.quick-dot {
  width: 8px;
  height: 8px;
  border-radius: 999px;
  background: #00a68e;
  box-shadow: 0 0 0 4px rgba(0, 166, 142, 0.1);
}

/* 全局可注入的 toast 样式 */
:global(.custom-toast-fail) {
  background-color: #f2fffb !important;
  color: #e77446 !important;
  font-size: 14px !important;
}
</style>
