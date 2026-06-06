# 设置页与健康档案页 UI 一致性改造 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 `setting` 目录下页面与 `record/Index.vue` 收敛到项目现有 `care` 风格体系，同时保留健康档案页主视觉。

**Architecture:** 以 `public/src/styles/index.css` 中已有的 `care-page`、`care-card`、`care-cell-group`、`care-primary-btn`、`care-danger-btn` 为统一视觉语义。设置页通过替换页面根容器、卡片容器与按钮语义实现统一；健康档案页保留数字人主视觉，只调整背景、底部功能卡、弹窗与文字层级。

**Tech Stack:** Vue 3、TypeScript、Vant 4、Tailwind CSS、UnoCSS、ESLint

---

### Task 1: 收敛设置首页与账号页

**Files:**
- Modify: `public/src/pages/setting/Index.vue`
- Modify: `public/src/pages/setting/Account.vue`

- [ ] **Step 1: 让设置首页头部卡片接入 `care-card`**

将 `setting/Index.vue` 中顶部说明块：

```vue
<div class="mb-4 rounded-[26px] bg-white/60 p-4">
  <p class="text-[13px] font-semibold text-[#00A68E]">账户与服务</p>
  <h1 class="mt-1 text-[22px] font-bold text-[#102A31]">管理你的健康数据体验</h1>
</div>
```

替换为：

```vue
<div class="care-card mb-4 p-4">
  <p class="text-[13px] font-semibold text-[#00A68E]">账户与服务</p>
  <h1 class="mt-1 text-[22px] font-bold text-[#102A31]">管理你的健康数据体验</h1>
  <p class="mt-2 text-sm text-[#6B7B86]">统一管理账号安全、反馈渠道与平台说明。</p>
</div>
```

- [ ] **Step 2: 将账号页切换到 `care-page` + `care-card` 结构**

把 `setting/Account.vue` 页面根容器和内容区替换为：

```vue
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
```

- [ ] **Step 3: 清理账号页冗余局部样式**

删除 `setting/Account.vue` 中仅用于旧渐变背景和旧导航配色的 `style scoped` 内容，仅保留必要的最小样式；如果没有必要样式，则删除整个 `style scoped`。

- [ ] **Step 4: 静态检查这两个页面**

确认以下内容未改变：

- 返回上一页逻辑仍调用 `router.back()`
- “修改密码”仍调用 `handleChangePassword`
- “注销账号”仍调用 `handleCancelAccount`

### Task 2: 收敛反馈页与关于页

**Files:**
- Modify: `public/src/pages/setting/Feedback.vue`
- Modify: `public/src/pages/setting/About.vue`

- [ ] **Step 1: 替换反馈页根容器与表单卡片**

将 `setting/Feedback.vue` 的核心模板替换为：

```vue
<div class="care-page flex min-h-screen flex-col">
  <van-nav-bar
    title="举报与反馈"
    left-arrow
    :border="false"
    class="transparent-nav"
    @click-left="onClickLeft"
  />

  <div class="mt-4 flex-1 px-4 pb-6">
    <div class="care-card mb-4 p-4">
      <p class="text-[13px] font-semibold text-[#00A68E]">产品反馈</p>
      <h1 class="mt-1 text-[22px] font-bold text-[#102A31]">告诉我们你的使用感受</h1>
      <p class="mt-2 text-sm text-[#6B7B86]">无论是问题反馈还是体验建议，我们都会认真记录并持续优化。</p>
    </div>

    <div class="care-card p-1">
      <van-field
        v-model="feedbackContent"
        rows="6"
        autosize
        type="textarea"
        maxlength="200"
        placeholder="请输入您的宝贵意见或反馈问题，我们将尽快处理（最多200字）"
        show-word-limit
        class="!bg-transparent"
      />
    </div>

    <van-button
      block
      round
      type="primary"
      class="care-primary-btn mt-6 !h-12"
      @click="handleSubmit"
    >
      提交反馈
    </van-button>
  </div>
</div>
```

- [ ] **Step 2: 删除反馈页单页按钮样式**

删除 `setting/Feedback.vue` 中：

```css
.neu-btn-primary {
  background: linear-gradient(135deg, #1890FF, #40A9FF) !important;
  border: none !important;
  box-shadow:
    4px 4px 10px rgba(24, 144, 255, 0.3),
    -4px -4px 10px rgba(255, 255, 255, 0.5) !important;
}
```

并移除仅用于旧导航配色的重复样式。

- [ ] **Step 3: 将关于页拆成统一卡片层级**

将 `setting/About.vue` 的主体内容整理为三段 `care-card`：

```vue
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
      <div class="flex h-20 w-20 items-center justify-center overflow-hidden rounded-3xl bg-white shadow-sm">
        <img :src="logoMark" alt="logo" class="h-22 w-32 object-fit" />
      </div>
      <p class="mt-4 text-xl font-bold text-[#102A31]">关于智医康</p>
      <p class="mt-2 text-sm text-[#6B7B86]">连接生命，赋能健康</p>
      <p class="mt-2 text-sm text-[#6B7B86]">Version 1.0.0</p>
    </div>

    <div class="care-card mt-4 p-5">
      <p class="text-base leading-7 text-[#4E5969]">
        海智医康科技有限公司是一家专注于慢病数字化疗法的前沿医疗科技企业。我们深度融合AI大模型能力与医疗级物联网硬件，通过结构化真实世界数据（RWD）赋能医生决策，提升患者生存质量与生存期。智医康创始团队由来自平安、哈啰、上海三甲医院的人工智能、医学、物联网专家组成，我们致力于帮助慢病患者科学康复，并坚持以循证医学为底座，助力“健康中国2030”建设。
      </p>
    </div>

    <div class="care-card mt-4 p-5 text-center text-xs text-[#6B7B86]">
      <p>© 2026 上海智医康科技有限公司 | 沪ICP备2024092528号-4 | 沪公网安备31010402336532号</p>
      <p class="mt-2">地址：上海市徐汇区龙漕路200弄28号302 | 邮箱：kefu@izencare.com | 电话：4006898982</p>
    </div>
  </div>
</div>
```

- [ ] **Step 4: 静态检查这两个页面**

确认以下行为不变：

- 反馈为空时仍提示 `请输入反馈内容`
- 提交成功后仍走原有成功提示与返回逻辑
- 关于页返回按钮仍正常可用

### Task 3: 收敛健康档案页视觉语义

**Files:**
- Modify: `public/src/pages/record/Index.vue`

- [ ] **Step 1: 替换页面根背景为 `care-page`**

将根节点：

```vue
<div class="min-h-screen bg-gradient-to-r from-[#e1f2fc] to-[#daf0fd] flex flex-col relative overflow-hidden">
```

替换为：

```vue
<div class="care-page relative flex min-h-screen flex-col overflow-hidden">
```

- [ ] **Step 2: 收敛顶部与名称区的文字色和卡片语义**

将顶部姓名区类名从偏单页蓝色体系调整为更贴近现有项目色板，例如：

```vue
<div class="absolute left-4 top-2 flex items-center rounded-r-full rounded-tl-full bg-gradient-to-r from-[#00A68E] to-[#2E84FF] p-2 pr-3 shadow-md">
```

姓名文字与编辑图标保持：

```vue
<span class="text-xl font-bold tracking-wider text-[#102A31]">{{ displayName }}</span>
<van-icon name="edit" class="ml-2 text-[#6B7B86]" size="18" @click="handleEditName" />
```

- [ ] **Step 3: 收敛底部功能卡容器**

将底部功能区容器：

```vue
<div class="w-full flex-1 bg-white/10 backdrop-blur-lg border border-white/20 rounded-t-[32px] pt-8 px-4 shadow-[0_-4px_20px_rgba(0,0,0,0.05)] z-30">
  <div class="grid grid-cols-4 gap-4 bg-white/95 p-4 rounded-xl">
```

替换为：

```vue
<div class="z-30 mt-4 w-full flex-1 rounded-t-[32px] border border-white/60 bg-white/35 px-4 pt-8 shadow-[0_-10px_30px_rgba(16,42,49,0.08)] backdrop-blur-xl">
  <div class="care-card grid grid-cols-4 gap-4 p-4">
```

并将图标占位块调整为：

```vue
<div class="flex h-14 w-14 items-center justify-center rounded-2xl bg-[#E7F6F2] shadow-inner">
```

功能文字调整为：

```vue
<span class="text-xs text-[#4E5969]">{{ item.name }}</span>
```

- [ ] **Step 4: 收敛姓名编辑弹窗**

将弹窗输入容器和按钮收敛为：

```vue
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
```

- [ ] **Step 5: 保持行为不变的静态检查**

确认以下逻辑未受影响：

- `handleEditName`
- `handleCloseEditPopup`
- `handleSaveName`
- `handleMenuClick`

### Task 4: 统一校验

**Files:**
- Modify: `public/src/pages/setting/Index.vue`
- Modify: `public/src/pages/setting/Account.vue`
- Modify: `public/src/pages/setting/Feedback.vue`
- Modify: `public/src/pages/setting/About.vue`
- Modify: `public/src/pages/record/Index.vue`

- [ ] **Step 1: 运行 ESLint**

Run:

```bash
cd d:\Vue3Project\ring_data\public
npm run lint
```

Expected:

```text
ESLint 通过，无新增错误
```

- [ ] **Step 2: 检查最近修改文件诊断**

使用编辑器诊断工具检查以下文件：

- `public/src/pages/setting/Index.vue`
- `public/src/pages/setting/Account.vue`
- `public/src/pages/setting/Feedback.vue`
- `public/src/pages/setting/About.vue`
- `public/src/pages/record/Index.vue`

期望结果：

- 无新增语法错误
- 无新增模板绑定错误
- 无新增样式块错误

- [ ] **Step 3: 提交实现变更**

```bash
git add public/src/pages/setting/Index.vue public/src/pages/setting/Account.vue public/src/pages/setting/Feedback.vue public/src/pages/setting/About.vue public/src/pages/record/Index.vue
git commit -m "feat: unify setting and record page ui"
```

如果当前工作区存在未准备处理的其他改动，则跳过提交，仅保留本地修改并在交付时说明。
