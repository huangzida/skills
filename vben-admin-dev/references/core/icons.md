# 图标使用

**官方文档**：https://doc.vben.pro/guide/advance/icon.html

**图标库**：https://icon-sets.iconify.design/

项目图标主要由 `@vben/icons` 包提供，建议统一在该包内部管理。

## 四种图标使用方式

### 1. 预定义图标组件（推荐）

对于频繁使用的图标，建议添加到 `@vben/icons` 包中。

#### 新增预定义图标

在 `packages/icons/src/iconify/index.ts` 目录下新增图标：

```typescript
// packages/icons/src/iconify/index.ts
import { createIconifyIcon } from '@vben-core/icons';

export const MdiKeyboardEsc = createIconifyIcon('mdi:keyboard-esc');
export const LucideCpu = createIconifyIcon('lucide:cpu');
export const LucideServer = createIconifyIcon('lucide:server');
```

#### 使用预定义图标

```vue
<script setup lang="ts">
import { LucideCpu, LucideServer } from '@vben/icons';
</script>

<template>
  <LucideCpu class="size-5" />
  <LucideServer class="size-5" />
</template>
```

**优点**：类型安全、智能提示、打包优化

### 2. IconifyIcon 组件

适用于动态图标或偶尔使用的图标。

```vue
<script setup lang="ts">
import { IconifyIcon } from '@vben/icons';

import { computed } from 'vue';

const props = defineProps<{
  status: 'success' | 'error' | 'warning';
}>();

const iconName = computed(() => {
  switch (props.status) {
    case 'success': return 'lucide:check-circle';
    case 'error': return 'lucide:x-circle';
    case 'warning': return 'lucide:alert-circle';
  }
});
</script>

<template>
  <IconifyIcon :icon="iconName" class="size-5" />
</template>
```

### 3. Svg 图标

没有采用 Svg Sprite 的方式，而是直接引入 Svg 图标。

#### 新增 Svg 图标

在 `packages/icons/src/svg/icons` 目录下新增图标文件 `test.svg`，然后在 `packages/icons/src/svg/index.ts` 中引入：

```typescript
// packages/icons/src/svg/index.ts
import { createIconifyIcon } from '@vben-core/icons';

const SvgTestIcon = createIconifyIcon('svg:test');

export { SvgTestIcon };
```

#### 使用 Svg 图标

```vue
<script setup lang="ts">
import { SvgTestIcon } from '@vben/icons';
</script>

<template>
  <SvgTestIcon class="size-5" />
</template>
```

### 4. Tailwind CSS 图标

官方支持的方式，直接添加 Tailwind CSS 的图标类名。**非常适合一次性使用或快速原型开发。**

```vue
<span class="icon-[lucide--cpu]"></span>
<span class="icon-[mdi--home]"></span>
<span class="icon-[ant-design--alipay-circle-outlined]"></span>
```

**优点**：无需导入、即写即用、适合一次性场景

**缺点**：无法享受类型检查、IDE 提示可能不完整

## 方式选择建议

| 场景 | 推荐方式 | 原因 |
|------|---------|------|
| 频繁使用的图标 | 预定义组件 | 类型安全、打包优化 |
| 动态图标 | IconifyIcon 组件 | 灵活、按需加载 |
| 自定义图标 | Svg 图标 | 高度定制化 |
| 一次性/快速开发 | Tailwind CSS | 即写即用、无需导入 |

## Tailwind 尺寸类

vben-admin 使用 Tailwind 扩展了尺寸类：

| 类名 | 等价于 |
|------|--------|
| `size-4` | `w-4 h-4` |
| `size-5` | `w-5 h-5` |
| `size-6` | `w-6 h-6` |
| `size-7` | `w-7 h-7` |
| `size-14` | `w-14 h-14` |

## 常用 Lucide 图标

```vue
<template>
  <IconifyIcon icon="lucide:home" />
  <IconifyIcon icon="lucide:user" />
  <IconifyIcon icon="lucide:settings" />
  <IconifyIcon icon="lucide:search" />
  <IconifyIcon icon="lucide:plus" />
  <IconifyIcon icon="lucide:pencil-line" />
  <IconifyIcon icon="lucide:trash-2" />
  <IconifyIcon icon="lucide:check" />
  <IconifyIcon icon="lucide:x" />
  <IconifyIcon icon="lucide:cpu" />
  <IconifyIcon icon="lucide:memory-stick" />
  <IconifyIcon icon="lucide:hard-drive" />
  <IconifyIcon icon="lucide:clock" />
  <IconifyIcon icon="lucide:server" />
  <IconifyIcon icon="lucide:monitor" />
  <IconifyIcon icon="lucide:tag" />
  <IconifyIcon icon="lucide:hash" />
  <IconifyIcon icon="lucide:bar-chart-3" />
</template>
```

## 菜单图标

```typescript
// 路由配置
meta: {
  icon: 'lucide:file-text',
}
```

## 按钮中使用图标

放在按钮的 `#icon` 插槽中：

```vue
<Button type="primary">
  <template #icon>
    <IconifyIcon icon="lucide:plus" class="size-4" />
  </template>
  添加
</Button>
```

## 常见问题

### Q: 为什么使用 @vben/icons 而不是 @iconify/vue？

A: `@vben/icons` 是 vben-admin 封装的图标包，重新导出 `@vben-core/icons` 的内容，并添加了预定义的 SVG 图标组件。使用统一入口便于维护。

### Q: 预定义组件和 IconifyIcon 组件如何选择？

A:
- **预定义组件**：频繁使用的图标，类型安全、打包优化
- **IconifyIcon 组件**：动态图标或偶尔使用的图标，更灵活

### Q: Tailwind CSS 图标有什么优势？

A: 适合快速开发、一次性场景，无需导入组件，直接使用类名即可。官方 playground 的 icons 示例中就有使用。

### Q: VSCode 开发推荐安装什么插件？

A: 推荐安装 **Iconify IntelliSense** 插件，可以方便地查找和使用图标，支持自动补全。

## 搜索图标

访问 [Iconify](https://icon-sets.iconify.design/) 搜索所需图标。
