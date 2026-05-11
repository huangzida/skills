# 视图切换 — Segmented 组件

> **场景**：列表/卡片/网格视图切换、Tab 式单选切换等。

## ⚠️ 强制规范

**禁止手写 `<button>` 按钮组实现视图切换。** 统一使用 Antdv Next `Segmented` 组件。

## 基础用法（仅图标）

适用于工具栏中的列表/网格视图切换：

```vue
<script lang="ts" setup>
import type { ViewMode } from './mock';

import { computed, h } from 'vue';

import { LayoutGrid, List } from '@vben/icons';
import { Segmented } from 'antdv-next';

const viewMode = defineModel<ViewMode>('viewMode', { default: 'list' });
</script>

<template>
  <Segmented
    v-model:value="viewMode"
    class="[&_svg]:size-4"
    :options="[
      { value: 'list', icon: h(List), tooltip: '列表视图' },
      { value: 'grid', icon: h(LayoutGrid), tooltip: '网格视图' },
    ]"
  />
</template>
```

## 带文字 + 图标

```vue
<Segmented
  v-model:value="viewMode"
  :options="[
    { value: 'list', label: '列表', icon: h(List) },
    { value: 'grid', label: '卡片', icon: h(LayoutGrid) },
  ]"
/>
```

## 使用 iconRender 插槽

当不想用 `h()` 渲染函数时，可通过插槽渲染图标：

```vue
<Segmented
  v-model:value="viewMode"
  :options="[
    { value: 'list' },
    { value: 'grid' },
  ]"
>
  <template #iconRender="{ value }">
    <List v-if="value === 'list'" class="size-4" />
    <LayoutGrid v-else class="size-4" />
  </template>
</Segmented>
```

## API 速查

### Props

| 属性 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `value` (v-model) | 当前选中值 | `string \| number` | - |
| `options` | 选项列表 | `SegmentedItemType[]` | `[]` |
| `size` | 尺寸 | `'small' \| 'middle' \| 'large'` | `'middle'` |
| `block` | 撑满父容器宽度 | `boolean` | `false` |
| `shape` | 形状 | `'default' \| 'round'` | `'default'` |
| `disabled` | 禁用全部选项 | `boolean` | `false` |

### SegmentedItemType

| 属性 | 说明 | 类型 |
|------|------|------|
| `value` | 选项值 | `string \| number` |
| `label` | 显示文字 | `VueNode` |
| `icon` | 显示图标（需 `h()` 包裹） | `VueNode` |
| `tooltip` | 悬停提示 | `string \| TooltipProps` |
| `disabled` | 禁用该选项 | `boolean` |

### Slots

| 插槽 | 说明 |
|------|------|
| `iconRender` | 自定义图标渲染 `{ value }` |
| `labelRender` | 自定义标签渲染 `{ label, value }` |

## 全局样式

`Segmented` 的图标居中样式已在全局 CSS 中配置（`packages/styles/src/antdv-next/index.css`），无需组件内额外处理。

```css
/* 已全局配置 */
.ant-segmented .ant-segmented-item-label {
  display: flex;
  align-items: center;
  justify-content: center;
}
```

如需控制图标尺寸，在组件上添加 `class="[&_svg]:size-4"` 即可。
