# ⚠️ Vben Admin 5.0 强制开发规范

> **本文件包含所有开发时必须遵守的硬性规则，违反这些规范将导致 lint 错误或运行时异常。**

---

## 🚨 1. 必须使用 vben 官方组件

> **严禁自己实现 Table 和 Modal！vben-admin 提供了完整的适配层，必须使用！**

| 功能 | ❌ 禁止使用 | ✅ 必须使用 |
|------|------------|-----------|
| 表格 | `antdv-next Table`、`html table` | `useVbenVxeGrid` from `#/adapter/vxe-table` |
| 弹窗 | `antdv-next Modal`、`antdv-next Drawer` | `useVbenModal` from `@vben/common-ui` |
| 表单 | 原生 Input/Select 拼装 | `useVbenForm` from `#/adapter/form` |
| 按钮 | `antdv-next Button`、原生 button | `VbenButton` from `@vben/common-ui` |

**违规示例：**
```vue
<!-- ❌ 错误：使用 antdv-next 的 Table 和 Modal -->
<template>
  <Table :columns="columns" :data-source="data" />
  <Button @click="openModal">打开</Button>
</template>

<!-- ❌ 错误：使用原生表单组件 -->
<template>
  <Input v-model="form.name" />
  <Select v-model="form.status" />
</template>
```

**正确示例：**
```vue
<!-- ✅ 正确：使用 vben 官方适配层 -->
<script setup lang="ts">
import { useVbenVxeGrid } from '#/adapter/vxe-table';
import { useVbenForm } from '#/adapter/form';
import { useVbenModal } from '@vben/common-ui';
import { VbenButton } from '@vben/common-ui';
</script>
```

---

## 🚨 2. 禁止使用非空断言 `!`

> **必须使用空值合并 `??` 或可选链 `?.`**

| 场景 | ❌ 禁止使用 | ✅ 正确使用 |
|------|------------|-----------|
| 访问可能为 undefined 的属性 | `data.triggerConditions!` | `data.triggerConditions ?? []` |
| 访问可能为 null 的值 | `value!.toString()` | `value?.toString() ?? ''` |

```typescript
// ❌ 错误
const conditions = data.triggerConditions!.length > 0
  ? data.triggerConditions! : defaultConditions;

// ✅ 正确
const conditions = (data.triggerConditions?.length ?? 0) > 0
  ? data.triggerConditions ?? [] : defaultConditions;
```

---

## 🚨 3. 图标必须使用 `@vben/icons`

> **禁止自行实现 SVG 或使用内联 SVG！**

| 场景 | ❌ 禁止使用 | ✅ 必须使用 |
|------|------------|-----------|
| 路由/菜单图标 | 自行编写 `<svg>` | `@vben/icons` 导出的 Lucide 图标 |
| 组件内图标 | 内联 SVG path | `@vben/icons` 导出的 Lucide/SVG 图标 |
| 统计卡片图标 | 手写 SVG path | `@vben/icons` 的 `SvgXxxIcon`（需 `markRaw`） |

**图标分类：**
- **Lucide 图标**（通用）：`import { Eye, Search, Settings, Package } from '@vben/icons'`
- **SVG 业务图标**（统计卡片等）：`import { SvgAlarmIcon, SvgDeviceIcon } from '@vben/icons'`

```vue
<!-- ✅ 正确 -->
<script setup lang="ts">
import { Eye, SvgDeviceIcon } from '@vben/icons';
import { markRaw } from 'vue';
</script>
<template>
  <Eye class="h-6 w-6" />
  <component :is="markRaw(SvgDeviceIcon)" class="h-6 w-6" />
</template>
```

**⚠️ 如果 `@vben/icons` 中没有所需图标：**
1. 优先查找已有的 Lucide 图标替代
2. 如确需新图标，在 `packages/@core/base/icons/src/lucide.ts` 或 `packages/icons/src/svg/index.ts` 中统一添加
3. 添加后从 `@vben/icons` 导出使用

---

## 🚨 4. 深拷贝必须使用 `structuredClone()`

> **禁止使用 `JSON.parse(JSON.stringify(...))`！**

```typescript
// ❌ 错误
const copy = JSON.parse(JSON.stringify(data));

// ✅ 正确
const copy = structuredClone(data);
```

---

## 🚨 5. 禁止在 componentProps 中使用 `style` 对象

> **会导致 `CSSStyleDeclaration` 运行时错误！使用 Tailwind `class` 替代。**

```typescript
// ❌ 错误：style 对象导致 CSSStyleDeclaration 错误
componentProps: { style: { width: '100%' } }

// ✅ 正确：使用 Tailwind class
componentProps: { class: 'w-full' }
```

---

## 🚨 6. `Promise.reject()` 必须包含 Error 对象

```typescript
// ❌ 错误
Promise.reject();

// ✅ 正确
Promise.reject(new Error('reason'));
```

---

## 🚨 7. 弹窗标题必须使用 computed 动态计算

> **共用 Create/Edit 弹窗时，禁止硬编码 title！**

```typescript
const modalData = ref<T | null>(null);
const isEdit = computed(() => !!modalData.value);
const modalTitle = computed(() =>
  isEdit.value ? $t('xxx.editTitle') : $t('xxx.createTitle'),
);

// ⚠️ modalApi.getData() 返回空对象 {} 而非 undefined
const hasData = data && Object.keys(data).length > 0;
```

---

## 🚨 8. 禁止添加代码注释

> **除非用户明确要求，否则不要添加任何代码注释。**

---

## 🚨 9. 组件必须拆分（>200行 或 ≥2个功能区块）

> **组件超过 200 行或包含多个独立功能区块时，必须拆分！**
>
> 详见：[组件拆分最佳实践](guides/component-splitting.md)

| 指标 | 阈值 | 动作 |
|------|------|------|
| 代码行数 | > 200 行 | 考虑拆分 |
| 功能区块 | ≥ 2 个独立区块 | 必须拆分 |
| 组件嵌套 | > 3 层 | 重构结构 |

**典型拆分模式：**

```
dashboard/
├── components/
│   ├── charts/          # 图表组件
│   │   └── DeviceDonutChart.vue
│   ├── cards/           # 统计卡片
│   │   └── OverviewCards.vue
│   └── list/            # 数据列表
│       └── AlertList.vue
├── index.vue            # 主组件（布局协调）
└── mock.ts              # Mock 数据
```

**数据管理策略：**
- 静态配置：组件内部定义
- 业务数据：通过 props 传递
- 事件回调：通过 emits 传递

---

## 🚨 10. `useVbenVxeGrid` 的 `gridOptions.height` 禁止设置为 `'auto'`

> **默认创建时不设置 height 属性，让表格自动适应容器高度。**

| 场景 | ❌ 禁止使用 | ✅ 正确使用 |
|------|------------|-----------|
| 表格高度 | `height: 'auto'` | 不设置 height 属性（默认自适应） |
| 固定高度 | ❌ 禁止 | 需要固定高度时设置具体数值 |

```typescript
// ❌ 错误：height: 'auto' 会导致布局问题
gridOptions: {
  height: 'auto',
}

// ✅ 正确：不设置 height，让表格自适应
gridOptions: {
  // height 属性不设置
}
```
