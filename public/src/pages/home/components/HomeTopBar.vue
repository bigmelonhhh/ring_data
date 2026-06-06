<script setup lang="ts">
import { ref } from 'vue'
import HomeDrawer from './HomeDrawer.vue'

const drawerRef = ref<InstanceType<typeof HomeDrawer> | null>(null)

/**
 * 触发抽屉菜单从左侧滑出显示
 * @returns 无返回结果
 */
function handleMenuClick(): void {
  drawerRef.value?.openDrawer()
}
</script>

<template>
  <header class="home-top-bar sticky top-0 z-50 px-4 pb-3 pt-3">
    <!-- 左侧菜单，入口 -->
    <div class="brand-pill cursor-pointer" @click="handleMenuClick">
      <span class="icon-dot">
        <van-icon name="wap-nav" size="18" color="#00A68E" />
      </span>
      <span class="text-[16px] font-bold text-[#102A31]">智医康</span>
    </div>

    <!-- 标题区 -->
    <div class="mode-switch">
      <span class="mode-active">对话</span>
      <span>硬件</span>
    </div>

    <!-- 右侧占位 -->
    <div class="right-tools">
      <van-icon name="weapp-nav" size="18" color="#102A31" />
      <van-icon name="clear" size="18" color="#102A31" />
    </div>

    <!-- 侧边栏抽屉组件 -->
    <HomeDrawer ref="drawerRef" />
  </header>
</template>

<style scoped>
.home-top-bar {
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: center;
  gap: 10px;
  background: rgba(247, 252, 250, 0.84);
  backdrop-filter: blur(16px);
}

.brand-pill{
   min-height: 36px;
}
.right-tools,
.mode-switch {
  min-height: 36px;
  border: 1px solid rgba(255, 255, 255, 0.72);
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.76);
  box-shadow: 0 10px 24px rgba(16, 42, 49, 0.06);
}

.brand-pill {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 4px 0px 4px 0px;
}

.icon-dot {
  display: inline-flex;
  width: 28px;
  height: 28px;
  align-items: center;
  justify-content: center;
  border-radius: 999px;
  background: rgba(0, 166, 142, 0.12);
}

.mode-switch {
  display: flex;
  align-items: center;
  justify-content: center;
  justify-self: center;
  gap: 4px;
  padding: 3px;
  color: #6b7b86;
  font-size: 13px;
  font-weight: 700;
}

.mode-switch span {
  display: inline-flex;
  min-width: 58px;
  justify-content: center;
  border-radius: 999px;
  padding: 6px 8px;
}

.mode-active {
  background: #102a31;
  color: #ffffff;
}

.right-tools {
  display: flex;
  width: 74px;
  align-items: center;
  justify-content: center;
  gap: 12px;
}

.brand-pill:active,
.right-tools:active {
  transform: translateY(1px);
}
</style>
