---
name: vben
description: Use when developing or maintaining vben-admin 5.0 projects, creating CRUD modules, looking up component APIs, or customizing themes. Includes form/table components, routing, permissions, and login integration.
---

# Vben Admin 5.0 开发指南

面向 vben-admin 5.0 项目的高级开发指南，整合官方文档和最佳实践。

## ⚠️ 强制开发规范

### 🚨 必须使用 vben 官方组件

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

### 🚨 TypeScript 类型规范

> **禁止使用非空断言 `!`，必须使用空值合并 `??` 或可选链 `?.`**

| 场景 | ❌ 禁止使用 | ✅ 正确使用 |
|------|------------|-----------|
| 访问可能为 undefined 的属性 | `data.triggerConditions!` | `data.triggerConditions ?? []` |
| 访问可能为 null 的值 | `value!.toString()` | `value?.toString() ?? ''` |

**违规示例：**
```typescript
// ❌ 错误：非空断言
const conditions = data.triggerConditions!.length > 0
  ? data.triggerConditions!
  : defaultConditions;
```

**正确示例：**
```typescript
// ✅ 正确：空值合并 + 可选链
const conditions = (data.triggerConditions?.length ?? 0) > 0
  ? data.triggerConditions ?? []
  : defaultConditions;
```

### 📋 弹窗标题最佳实践

> **共用 Create/Edit 弹窗时，必须使用 `computed` 动态计算标题！**

当一个弹窗组件同时支持新增和编辑时，标题需要根据当前模式动态切换。正确做法：

```typescript
const modalData = ref<T | null>(null);
const isEdit = computed(() => !!modalData.value);
const modalTitle = computed(() =>
  isEdit.value ? $t('xxx.editTitle') : $t('xxx.createTitle'),
);

const [Modal, modalApi] = useVbenModal({
  // ⚠️ 不要在这里设置固定 title
});

function open(data?: T) {
  if (data) {
    modalData.value = data;
    modalApi.setData(data);
  } else {
    modalData.value = null;
    modalApi.setData(null);  // 注意：会设置空对象 {}
  }
  modalApi.open();
}
```

**模板中使用 `:title` 绑定：**
```vue
<template>
  <Modal :title="modalTitle">
    <!-- ... -->
  </Modal>
</template>
```

**⚠️ 关键注意事项：`modalApi.getData()` 返回空对象 `{}` 而非 `undefined`**

在 `onOpenChange` 中判断数据是否存在时：
```typescript
// ❌ 错误：空对象 {} 是 truthy
if (data) { ... }

// ✅ 正确：检查对象是否有属性
const hasData = data && Object.keys(data).length > 0;
if (hasData) { ... }
```

### 📋 CRUD 模块标准结构

```
views/
├── your-feature/
│   ├── index.vue           # 主页面（useVbenVxeGrid）
│   ├── data.ts            # 列定义（useColumns）
│   ├── mock.ts            # 模拟数据（可选）
│   └── components/
│       ├── CreateModal.vue    # 创建弹窗（useVbenForm + useVbenModal）
│       ├── EditModal.vue      # 编辑弹窗（useVbenForm + useVbenModal）
│       └── DetailDrawer.vue   # 详情抽屉（useVbenForm + useVbenDrawer）
```

### 📋 弹窗表单标准结构

```vue
<!-- components/CreateModal.vue -->
<script setup lang="ts">
import type { VbenFormSchema } from '#/adapter/form';
import { useVbenModal } from '@vben/common-ui';
import { useVbenForm, z } from '#/adapter/form';

const emit = defineEmits<{ submit: [payload: any] }>();

const schema: VbenFormSchema[] = [
  { fieldName: 'name', label: '名称', component: 'Input', rules: z.string().min(1, '请输入') },
  // ... 更多字段
];

const [Form, formApi] = useVbenForm({
  layout: 'vertical',
  schema,
  showDefaultActions: false,
});

const [Modal, modalApi] = useVbenModal({
  async onConfirm() {
    const { valid } = await formApi.validate();
    if (!valid) return;

    modalApi.lock();
    try {
      const values = await formApi.getValues();
      emit('submit', values);
      modalApi.close();
    } finally {
      modalApi.lock(false);
    }
  },
  onOpenChange(isOpen) {
    if (isOpen) formApi.resetForm();
  },
  title: '创建',
});

defineExpose({
  open: () => modalApi.open(),
});
</script>

<template>
  <Modal>
    <Form class="mx-4" />
  </Modal>
</template>
```

---

## 🔧 组件拆分规范

> **组件超过 200 行或包含多个功能区块时必须拆分！**

保持组件颗粒度细、易于维护和扩展：
- 将大组件拆分为多个小组件
- 每个组件职责单一
- 相关功能抽离为独立组件

| 场景 | 拆分方式 |
|------|----------|
| 表单 + 弹窗 | 使用 `useVbenForm` + `useVbenModal` |
| 复杂表单 | 按业务模块拆分为多个子表单 |
| 编辑详情 | 基本信息 + 物模型 等模块拆分 |
| 仪表板 | Header + 卡片组 + 图表 + 列表 等区块拆分 |

> 💡 **详细指南：** [组件拆分最佳实践](references/guides/component-splitting.md)

## 快速导航

### 🔰 入门

| 主题 | 文件 |
|------|------|
| [快速开始](references/guides/quick-start.md) | 环境准备、启动项目、创建页面 |
| [项目结构](references/guides/project-structure.md) | Monorepo 结构、路径别名 |
| [Playground 示例](references/guides/playground-index.md) | 表单、表格、弹窗示例索引 |
| [组件拆分](references/guides/component-splitting.md) | 仪表板/卡片等组件拆分最佳实践 |

### 📦 核心组件

| 组件 | 文件 | 特性 |
|------|------|------|
| [表单](references/components/form.md) | useVbenForm | valueFormat、fieldMappingTime、dependencies、handleValuesChange |
| [表格](references/components/table.md) | useVbenVxeGrid | treeConfig、editConfig、virtual scroll、自定义渲染器 |
| [图表](references/components/echarts.md) | EchartsUI + useEcharts | `dashboard/analytics/` |
| [弹窗/抽屉](references/components/modal-drawer.md) | useVbenModal/Drawer | lock/unlock、connectedComponent、appendToMain |
| [页面](references/components/page.md) | Page | `examples/page/` |
| [自定义组件](references/core/adapter.md) | InputNumberRange / IpInput / MacInput / TreeSelectWithCustomInput / Popconfirm / ApiComponent | `#/adapter/component` | ApiComponent 远程数据、beforeFetch/afterFetch、autoSelect |

### 🏗️ 核心功能

| 功能 | 文件 |
|------|------|
| [路由配置](references/core/route.md) | 路由定义、元信息、动态路由 |
| [权限控制](references/core/access.md) | 前端/后端模式、组件级权限 |
| [API请求](references/core/api.md) | 请求封装、拦截器 |
| [国际化](references/core/internationalization.md) | 语言包、切换语言 |
| [状态管理](references/core/state-management.md) | Store、Auth Store |
| [主题配置](references/core/theme.md) | 内置主题、自定义主题 |
| [样式设计](references/core/styling.md) | Tailwind 主题类、深色模式、语义颜色 |
| [图标使用](references/core/icons.md) | Iconify、常用图标集 |
| [登录对接](references/core/login.md) | 登录接口、Token刷新 |
| [适配器](references/core/adapter.md) | 组件适配、自定义注册 |

### ⚙️ 配置与定制

| 主题 | 文件 |
|------|------|
| [偏好设置](references/features/preferences.md) | preferences.ts 完整配置 |
| [CRUD生成](references/guides/crud-generation.md) | 标准CRUD开发流程 |
| [构建部署](references/guides/build-deploy.md) | 构建命令、部署配置 |

### 🎯 最佳实践

| 主题 | 文件 | 说明 |
|------|------|------|
| [文件下载](references/features/download.md) | requestClient.download() | 绕过响应拦截器 |
| [Dropdown/Popconfirm](references/components/dropdown-popconfirm.md) | 事件处理规则 | 避免重复触发 |

## 常用命令

```bash
# 开发
pnpm dev:playground   # Playground（推荐）

# 检查
pnpm check:type       # 类型检查
pnpm lint             # ESLint

# 构建
pnpm build            # 构建所有
```

## 官方资源

- [官方文档](https://doc.vben.pro)
- [GitHub](https://github.com/vbenjs/vue-vben-admin)
- [Iconify 图标](https://icon-sets.iconify.design/)
- [ECharts](https://echarts.apache.org/zh/index.html)

## 目录结构

```
vben/
├── SKILL.md                    # 主入口
├── README.md                   # 快速入门
├── references/
│   ├── components/             # 组件文档
│   ├── core/                   # 核心功能
│   ├── features/              # 配置定制
│   └── guides/                 # 开发指南
│       └── component-splitting.md  # 组件拆分最佳实践
```
