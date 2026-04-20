# 图标使用

**官方文档**：https://doc.vben.pro/guide/advance/icon.html

**图标库**：https://icon-sets.iconify.design/

项目图标主要由 `@vben/icons` 包提供，建议统一在该包内部管理。

## ⚡ 优先推荐：离线图标方案

**🚫 避免使用 `IconifyIcon` 组件加载远程图标** - 会发起网络请求（如 `https://api.iconify.design/lucide.json`），不支持离线环境。

**✅ 推荐使用本地 lucide-vue-next 图标** - 打包在应用中，无需网络请求，完全支持离线。

## 四种图标使用方式

### 1. 预定义本地图标组件 ⭐ 首选

对于频繁使用的图标，**强烈推荐使用本地 lucide-vue-next 图标**，通过 `@vben/icons` 包统一管理。

#### 新增预定义图标

在 `packages/@core/base/icons/src/lucide.ts` 或 `packages/icons/src/iconify/index.ts` 中新增：

```typescript
// packages/@core/base/icons/src/lucide.ts (推荐 - 使用 lucide-vue-next)
import { createIconifyIcon } from '@vben-core/icons';
import * as lucideIcons from 'lucide-vue-next';

export const LucideCpu = lucideIcons.Cpu;
export const LucideServer = lucideIcons.Server;
```

```typescript
// packages/icons/src/iconify/index.ts (备选 - 使用 createIconifyIcon)
import { createIconifyIcon } from '@vben-core/icons';

export const MdiKeyboardEsc = createIconifyIcon('mdi:keyboard-esc');
export const LucideCpu = createIconifyIcon('lucide:cpu');
export const LucideServer = createIconifyIcon('lucide:server');
```

#### 使用预定义图标

```vue
<script setup lang="ts">
import { Cpu, Server } from '@vben/icons';
</script>

<template>
  <Cpu class="size-5" />
  <Server class="size-5" />
</template>
```

**优点**：✅ 离线可用、✅ 类型安全、✅ 智能提示、✅ 打包优化、✅ 无网络请求

### 2. IconifyIcon 组件（谨慎使用）

**⚠️ 注意**：此方式会发起网络请求获取图标定义，仅在以下情况使用：
- 需要使用 `@vben/icons` 包中未预定义的非 lucide 图标
- 动态图标场景且确定应用可联网

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

**⚠️ 警告**：使用 `lucide:` 前缀的 IconifyIcon 仍会发起网络请求，建议优先使用预定义组件。

### 3. Svg 图标

适用于自定义业务图标，完全本地化。

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

### 4. Tailwind CSS 图标（不推荐）

**⚠️ 不推荐**：虽然官方支持，但会发起网络请求，不适合离线环境。

```vue
<span class="icon-[lucide--cpu]"></span>
<span class="icon-[mdi--home]"></span>
<span class="icon-[ant-design--alipay-circle-outlined]"></span>
```

**缺点**：❌ 网络请求、❌ 无法享受类型检查、❌ IDE 提示不完整

## 方式选择建议

| 场景 | 推荐方式 | 原因 | 离线支持 |
|------|---------|------|---------|
| 频繁使用的图标 | **预定义组件** | ✅ 类型安全、✅ 打包优化 | ✅ 完全支持 |
| 常用 Lucide 图标 | **lucide-vue-next** | ✅ 离线可用、✅ 无网络请求 | ✅ 完全支持 |
| 自定义图标 | **Svg 图标** | ✅ 完全本地化 | ✅ 完全支持 |
| 动态图标（偶尔） | IconifyIcon | ⚠️ 灵活但有网络依赖 | ❌ 需要网络 |
| 快速原型 | Tailwind CSS | ⚠️ 即写即用但不推荐 | ❌ 需要网络 |

## Tailwind 尺寸类

vben-admin 使用 Tailwind 扩展了尺寸类：

| 类名 | 等价于 |
|------|--------|
| `size-4` | `w-4 h-4` |
| `size-5` | `w-5 h-5` |
| `size-6` | `w-6 h-6` |
| `size-7` | `w-7 h-7` |
| `size-14` | `w-14 h-14` |

## 常用 Lucide 图标（离线可用）

```vue
<template>
  <Home class="size-4" />
  <User class="size-4" />
  <Settings class="size-4" />
  <Search class="size-4" />
  <Plus class="size-4" />
  <PencilLine class="size-4" />
  <Trash2 class="size-4" />
  <Check class="size-4" />
  <X class="size-4" />
  <Cpu class="size-4" />
  <MemoryStick class="size-4" />
  <HardDrive class="size-4" />
  <Clock class="size-4" />
  <Server class="size-4" />
  <Monitor class="size-4" />
  <Tag class="size-4" />
  <Hash class="size-4" />
  <BarChart3 class="size-4" />
  <Download class="size-4" />
  <Upload class="size-4" />
  <FolderTree class="size-4" />
  <LayoutGrid class="size-4" />
  <List class="size-4" />
  <ImagePlus class="size-4" />
  <Package class="size-4" />
</template>
```

## 菜单图标

```typescript
// 路由配置 - 使用预定义的 Lucide 图标名称
meta: {
  icon: 'lucide:file-text',
}
```

## 按钮中使用图标

放在按钮的 `#icon` 插槽中：

```vue
<Button type="primary">
  <template #icon>
    <Plus class="size-4" />
  </template>
  添加
</Button>
```

## 常见问题

### Q: 为什么优先推荐离线图标方案？

A: 
- **离线支持**：工业控制、企业内网等场景需要完全离线运行
- **性能优化**：避免网络请求，加快首次加载速度
- **稳定性**：不依赖外部服务，避免图标加载失败
- **用户体验**：在弱网环境下也能正常显示图标

### Q: 如何检查代码是否使用了网络图标？

A: 搜索以下模式：
```bash
# 检查 IconifyIcon 使用
grep -r "IconifyIcon.*icon=" src/

# 检查 lucide: 前缀（会发起网络请求）
grep -r "lucide:" src/
```

### Q: 如何迁移现有 IconifyIcon 到本地图标？

A: 
1. 查找使用的图标名称（如 `lucide:plus`）
2. 在 `packages/@core/base/icons/src/lucide.ts` 中导出对应图标
3. 替换 `<IconifyIcon icon="lucide:plus" />` 为 `<Plus />`

### Q: 预定义组件和 IconifyIcon 组件如何选择？

A:
- **✅ 预定义组件（lucide-vue-next）**：所有场景首选，离线可用
- **IconifyIcon 组件**：仅在需要使用未预定义的图标时谨慎使用

### Q: Tailwind CSS 图标有什么优势？

A: **无优势**。虽然即写即用，但会发起网络请求，不推荐在正式项目中使用。

### Q: VSCode 开发推荐安装什么插件？

A: 推荐安装 **Iconify IntelliSense** 插件，可以方便地查找和使用图标，支持自动补全。

## 搜索图标

访问 [Iconify](https://icon-sets.iconify.design/) 或 [Lucide](https://lucide.dev/) 搜索所需图标。

**💡 建议**：优先在 Lucide 图标库搜索，通常能满足大部分需求且完全离线可用。
