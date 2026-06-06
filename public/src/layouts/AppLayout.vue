<template>
  <div class="h-screen overflow-hidden app-layout-bg">
    <main class="h-full">
      <router-view v-slot="{ Component }">
        <transition name="page-slide" mode="out-in">
          <component :is="Component" />
        </transition>
      </router-view>
    </main>
  </div>
</template>

<style scoped>
.app-layout-bg {
  background:
    radial-gradient(circle at 18% 10%, rgba(104, 225, 205, 0.26), transparent 28%),
    linear-gradient(180deg, #f6fffc 0%, #eef8f5 56%, #f7fbff 100%);
}

.page-slide-enter-active,
.page-slide-leave-active {
  transition:
    transform 0.2s ease,
    opacity 0.2s ease;
}

.page-slide-enter-from,
.page-slide-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
