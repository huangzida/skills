# Vben Admin 样式系统

Vben Admin 内置完整的 Tailwind CSS 主题系统，提供语义化颜色类和深色模式支持，无需手动编写 CSS 即可适配主题。

## 主题系统概览

Vben Admin 使用 **shadcn/ui** 风格的语义化颜色系统，所有颜色通过 CSS 变量定义，支持主题切换和深色模式。

### 核心文件

- **主题变量**: `/internal/tailwind-config/src/theme.css`
- **颜色定义**: 各 UI Kit 包中的 `tokens.ts`

### 深色模式

Vben Admin 使用 `.dark` 类选择器实现深色模式，**不是** `prefers-color-scheme`媒体查询：

```css
@custom-variant dark (&:is(.dark *));
```

所有语义颜色类都会自动适配深色模式。

## 语义颜色系统

### 基础语义颜色 (shadcn/ui)

| 类别 | 变量 | 说明 | 用途 |
|------|------|------|------|
| **背景** | `--color-background` | 主背景色 | 页面、卡片背景 |
| **前景** | `--color-foreground` | 主文本色 | 文字、图标 |
| **卡片** | `--color-card` / `--color-card-foreground` | 卡片背景/文字 | 独立卡片组件 |
| **弹出层** | `--color-popover` / `--color-popover-foreground` | 弹窗背景 | 下拉菜单、弹出框 |
| **次要** | `--color-secondary` / `--color-secondary-foreground` | 次要背景 | 次要区域、标签 |
| **强调** | `--color-accent` / `--color-accent-foreground` | 强调背景 | 高亮区域、hover |
| **静音** | `--color-muted` / `--color-muted-foreground` | 静音背景 | 禁用状态、提示 |
| **边框** | `--color-border` / `--color-input` | 边框/输入框 | 分隔线、输入框边框 |
| **环形** | `--color-ring` | 焦点环 | 焦点状态指示 |

### 自定义语义颜色

| 变量 | 说明 | 用途 |
|------|------|------|
| `--color-header` | 头部区域 | 顶部导航栏 |
| `--color-sidebar` / `--color-sidebar-deep` | 侧边栏 | 菜单区域 |
| `--color-main` | 主内容区 | 主要内容背景 |
| `--color-heavy` / `--color-heavy-foreground` | 重要区域 | 突出显示区域 |
| `--color-overlay` / `--color-overlay-content` | 遮罩层 | 模态框背景 |

## 功能颜色调色板

### Primary 主色调

完整调色板：`primary-50` → `primary-700`

```html
<!-- 背景类 -->
<div class="bg-primary">主色背景</div>
<div class="bg-primary-100">浅主色背景</div>
<div class="bg-primary-700">深主色背景</div>
<div class="bg-primary-background-light">浅主色背景（浅10%）</div>
<div class="bg-primary-background-lighter">更浅主色背景</div>

<!-- 文字类 -->
<div class="text-primary">主色文字</div>
<div class="text-primary-text">主色文字（浅）</div>
<div class="text-primary-text-hover">主色文字（hover）</div>
<div class="text-primary-text-active">主色文字（active）</div>

<!-- 边框类 -->
<div class="border-primary">主色边框</div>
<div class="border-primary-light">浅主色边框</div>

<!-- 交互状态 -->
<div class="hover:bg-primary-hover">Hover 状态</div>
<div class="active:bg-primary-active">Active 状态</div>
```

### 状态颜色

| 状态 | 主类 | 浅色变体 | 文字变体 | 边框变体 |
|------|------|----------|----------|----------|
| **成功** | `bg-success` | `bg-success-background-light` | `text-success-text` | `border-success` |
| **警告** | `bg-warning` | `bg-warning-background-light` | `text-warning-text` | `border-warning` |
| **危险** | `bg-danger` / `bg-destructive` / `bg-error` | `bg-danger-background-light` | `text-danger-text` | `border-danger` |

### 颜色别名

为了兼容性和语义化，还提供了颜色别名：

| 别名 | 来源 | 用途 |
|------|------|------|
| `green-*` | success | 表示成功/通过 |
| `red-*` | destructive | 表示错误/删除 |
| `yellow-*` | warning | 表示警告/待处理 |
| `danger-*` | destructive | 表示危险/删除操作 |
| `error-*` | destructive | 表示错误/失败状态 |

> **注意**：`danger-*` 和 `error-*` 是 `destructive-*` 的别名，内部都指向相同的颜色变量。

```html
<!-- 使用别名 -->
<div class="bg-green-100 text-green-text-hover">成功状态</div>
<div class="bg-red-50 text-red-text">错误提示</div>
<div class="bg-yellow-200 border-yellow-border">警告提示</div>
```

## 组件样式类

### 卡片组件

```html
<div class="card-box">
  <!-- 卡片内容，自动应用 bg-card border-border rounded-xl -->
</div>
```

### 链接样式

```html
<a class="vben-link">
  <!-- 自动应用 text-primary hover:text-primary-hover active:text-primary-active -->
</a>
```

### 布局工具类

| 类名 | 效果 |
|------|------|
| `flex-center` | `display: flex; align-items: center; justify-content: center;` |
| `flex-col-center` | `display: flex; flex-direction: column; align-items: center; justify-content: center;` |

### 轮廓框

```html
<div class="outline-box">
  <!-- 带轮廓交互的容器，hover 时显示边框 -->
</div>
```

## Tailwind-First 原则

Vben Admin 遵循 **Tailwind-First** 原则，优先使用 Tailwind CSS 类，避免使用 `<style scoped>`。

### ✅ 正确做法

```vue
<template>
  <!-- 布局和间距用 Tailwind -->
  <div class="flex items-center justify-between gap-4 p-5">
    <!-- 颜色和状态用语义类 -->
    <div class="bg-card border border-border rounded-xl p-4">
      <button class="h-10 px-4 font-semibold text-warning border border-warning/50 rounded-xl hover:bg-warning/10 transition-colors">
        按钮
      </button>
    </div>
  </div>
</template>
```

### ❌ 错误做法：大量自定义 CSS

```vue
<!-- ❌ 不推荐：把 Tailwind 能实现的效果写成 CSS -->
<template>
  <div class="custom-card">
    <button class="custom-button">按钮</button>
  </div>
</template>

<style scoped>
.custom-card {
  background-color: var(--color-card);
  border: 1px solid var(--color-border);
  border-radius: 0.75rem;
  padding: 1rem;
}

.custom-button {
  height: 2.5rem;
  padding: 0 1rem;
  font-weight: 600;
  color: var(--color-warning);
  border: 1px solid rgba(245, 158, 11, 0.5);
  border-radius: 0.75rem;
  transition: all 0.2s ease;
}

.custom-button:hover {
  background-color: rgba(245, 158, 11, 0.1);
}
</style>
```

### 何时使用 `<style scoped>`

只有 Tailwind 无法实现时才使用：

| 场景 | 解决方案 |
|------|----------|
| 动态 `min-height`（如图表容器） | ✅ 可用 `min-h-[400px]` Tailwind |
| CSS 动画关键帧 | ❌ 需要 `<style>` |
| 伪元素样式 | ⚠️ 可用 Tailwind `before:`/`after:` |
| CSS 变量注入 | ⚠️ 可用 `[--variable:value]` |
| `:deep()` 覆盖第三方组件样式 | ❌ 需要 `<style scoped>` 中的 `:deep()` |

### 何时使用 `:deep()`

需要覆盖第三方组件（如 ant-design-vue、element-plus）内部样式时使用：

```vue
<!-- ✅ 正确：使用 :deep() 覆盖第三方组件内部样式 -->
<style scoped>
:deep(.ant-tree-node-content-wrapper) {
  min-height: 36px;
  padding: 6px 10px;
}

:deep(.ant-tree-switcher) {
  display: flex;
  align-items: center;
  width: 4px;
}
</style>

<!-- ❌ 错误：为普通元素写自定义 CSS 类 -->
<style scoped>
.my-custom-div {
  display: flex;
  align-items: center;
  padding: 8px;
  border-radius: 12px;
}
</style>

<!-- ✅ 正确：用 Tailwind 类替代 -->
<div class="flex items-center p-2 rounded-xl">...</div>
```

### :deep() 使用原则

| 场景 | 是否使用 :deep() |
|------|------------------|
| 覆盖 ant-design-vue / element-plus 等组件内部样式 | ✅ 需要 |
| 覆盖第三方 UI 库的样式 | ✅ 需要 |
| 自定义组件样式 | ❌ 用 Tailwind 类 |
| 页面布局、间距、颜色 | ❌ 用 Tailwind 类 |
| 通用样式（flex、gap、padding 等） | ❌ 用 Tailwind 类 |

### 迁移指南

将现有 `<style scoped>` 迁移到 Tailwind：

```vue
<!-- 之前 -->
<div class="stat-card">...</div>

<style scoped>
.stat-card {
  @apply bg-card border border-border;
  box-shadow: 0 1px 2px 0 rgb(0 0 0 / 3%);
}
</style>

<!-- 之后 -->
<div class="stat-card bg-card border border-border shadow-[0_1px_2px_0_rgb(0_0_0_/_3%)]">...</div>
```

## 最佳实践

### ✅ 推荐：使用语义颜色类

```vue
<template>
  <!-- 页面主容器 -->
  <div class="bg-background text-foreground">
    <!-- 卡片 -->
    <div class="bg-card text-card-foreground rounded-xl border border-border p-4">
      <h2 class="text-primary font-semibold">标题</h2>
      <p class="text-muted-foreground">次要文本</p>
    </div>

    <!-- 按钮 -->
    <button class="bg-primary text-primary-foreground hover:bg-primary-hover">
      主要操作
    </button>

    <!-- 次要按钮 -->
    <button class="bg-secondary text-secondary-foreground">
      次要操作
    </button>

    <!-- 危险操作 -->
    <button class="bg-destructive text-destructive-foreground hover:bg-destructive-hover">
      删除
    </button>
  </div>
</template>
```

### ❌ 避免：硬编码颜色

```vue
<!-- ❌ 不推荐：硬编码 hex/rgb 颜色 -->
<template>
  <div class="bg-white text-gray-900">
    <div class="bg-[#f9fafb] border-[#e5e7eb]">
      <!-- 这些颜色不会随主题切换 -->
    </div>
  </div>
</template>

<!-- ✅ 推荐：使用语义颜色 -->
<template>
  <div class="bg-background text-foreground">
    <div class="bg-muted border-border">
      <!-- 自动适配主题和深色模式 -->
    </div>
  </div>
</template>
```

### 🎨 颜色使用场景

| 场景 | 推荐颜色类 |
|------|-----------|
| 页面背景 | `bg-background` |
| 主要内容 | `bg-main` |
| 侧边栏 | `bg-sidebar` |
| 卡片/面板 | `bg-card` / `bg-popover` |
| 主要文本 | `text-foreground` |
| 次要文本 | `text-muted-foreground` |
| 主要按钮 | `bg-primary` |
| 次要按钮 | `bg-secondary` |
| 危险操作 | `bg-destructive` |
| 成功状态 | `bg-success` / `text-success` |
| 警告状态 | `bg-warning` / `text-warning` |
| 边框/分隔 | `border-border` |
| 禁用/静音 | `bg-muted` / `text-muted-foreground` |

### 深色模式适配

```vue
<template>
  <!-- 基础组件 - 自动适配深色模式 -->
  <div class="bg-card border-border p-4 rounded-lg">
    <span class="text-foreground">自动适配</span>
  </div>

  <!-- 需要特殊深色模式样式时 -->
  <div class="bg-background dark:bg-sidebar-deep">
    <!-- 浅色模式用 background，深色模式用 sidebar-deep -->
  </div>

  <!-- 使用 dark: 前缀覆盖特定样式 -->
  <div class="bg-card dark:bg-overlay">
    <!-- 浅色模式卡片背景，深色模式遮罩背景 -->
  </div>
</template>
```

### 阴影和圆角

使用主题变量定义的高级变量：

```vue
<template>
  <!-- 圆角 - 根据 radius 配置自动缩放 -->
  <div class="rounded-sm">小圆角</div>
  <div class="rounded-md">中等圆角</div>
  <div class="rounded-lg">大圆角（默认）</div>
  <div class="rounded-xl">特大圆角</div>

  <!-- 浮动阴影 - 用于悬浮元素 -->
  <div class="shadow-float">
    <!-- 模态框、卡片悬浮状态 -->
  </div>
</template>
```

## 主题切换

### 切换深色模式

```typescript
import { useTheme } from '#/stores/theme';

const themeStore = useTheme();

// 切换深色模式
themeStore.toggleDarkMode();

// 设置特定模式
themeStore.setTheme('light');
themeStore.setTheme('dark');
themeStore.setTheme('auto');
```

### 自定义主题颜色

在 `preferences.ts` 中配置：

```typescript
import { defineGlobalConfig } from '#/definitions';

export const globalSettings: DefineGlobalSettings = {
  theme: {
    // 主色调
    primaryColor: '#3b82f6',
    // 成功色
    successColor: '#22c55e',
    // 警告色
    warningColor: '#f59e0b',
    // 危险色
    destructiveColor: '#ef4444',
    // 圆角大小
    radius: 0.5, // rem
  },
};
```

## 工具函数

### 使用 CSS 变量

```typescript
import { useDesign } from '/@/hooks/useDesign';

const { prefixCls } = useDesign('component');

// 生成带前缀的类名
const wrapperClass = `${prefixCls}-wrapper`; // vben-component-wrapper
```

### 动态主题颜色

```typescript
import { useDesign } from '/@/hooks/useDesign';

const { cssVariable } = useDesign('primary');

// 生成 CSS 变量
const color = cssVariable('--primary'); // var(--primary)
```

## 动画类

### 入场动画

| 类名 | 效果 | 用法 |
|------|------|------|
| `enter-x` | 从右侧滑入 | 列表项依次入场 |
| `-enter-x` | 从左侧滑入 | 列表项依次入场 |
| `enter-y` | 从下方滑入 | 列表项依次入场 |
| `-enter-y` | 从上方滑入 | 列表项依次入场 |

```html
<div class="flex gap-4">
  <div class="enter-x">项目 1</div>
  <div class="enter-x">项目 2</div>
  <div class="enter-x">项目 3</div>
</div>
```

### 浮动动画

```html
<div class="animate-float">
  <!-- 持续上下浮动动画 -->
</div>
```

## 示例：完整卡片组件

```vue
<template>
  <div class="bg-card border border-border rounded-xl p-6 shadow-float">
    <div class="flex items-center justify-between mb-4">
      <h3 class="text-foreground text-lg font-semibold">卡片标题</h3>
      <span class="bg-primary-100 text-primary-foreground px-3 py-1 rounded-full text-sm">
        标签
      </span>
    </div>

    <p class="text-muted-foreground mb-4">
      卡片描述文本，使用 muted-foreground 颜色。
    </p>

    <div class="flex gap-3">
      <button class="bg-primary text-primary-foreground hover:bg-primary-hover px-4 py-2 rounded-lg">
        主要操作
      </button>
      <button class="bg-secondary text-secondary-foreground hover:bg-accent-hover px-4 py-2 rounded-lg">
        次要操作
      </button>
      <button class="bg-destructive text-destructive-foreground hover:bg-destructive-hover px-4 py-2 rounded-lg">
        删除
      </button>
    </div>
  </div>
</template>
```

## 相关资源

- [Tailwind CSS 文档](https://tailwindcss.com/docs)
- [shadcn/ui 颜色系统](https://ui.shadcn.com/docs/theming)
- [主题配置](../features/theme.md)
