# 项目结构

## Monorepo 结构

```
api-admin/
├── apps/              # 应用目录
│   ├── playground/   # 示例和演示 ⭐
│   ├── web-antd/     # Ant Design Vue
│   ├── web-ele/      # Element Plus
│   └── web-naive/    # Naive UI
├── packages/         # 共享包
│   ├── @core/        # 核心包
│   │   ├── base/     # 基础包
│   │   ├── ui-kit/   # UI 组件包
│   │   └── layouts/  # 布局包
│   ├── effects/       # 功能包
│   │   ├── access/   # 权限控制
│   │   ├── hooks/    # 钩子函数
│   │   └── layouts/  # 布局功能
│   ├── stores/       # 状态管理
│   ├── locales/      # 国际化
│   └── utils/        # 工具函数
├── docs/             # 文档
└── internal/         # 内部工具配置
```

## Playground 结构

```
apps/playground/src/
├── adapter/          # 组件适配器
├── api/              # API 接口
├── components/       # 公共组件
├── hooks/            # 组合式函数
├── locales/          # 国际化
├── router/           # 路由配置
├── stores/           # 状态管理
├── styles/           # 全局样式
├── utils/            # 工具函数
└── views/            # 页面视图
    ├── _core/        # 核心页面
    ├── demos/        # 演示
    └── examples/     # 示例
```

## 路径别名

| 别名 | 路径 |
|------|------|
| `#/` | `src/` |
| `#@` | 源代码根目录 |
| `#/api` | `src/api` |
| `#/adapter` | `src/adapter` |
| `#/locales` | `src/locales` |
| `#/store` | `src/store` |

## 业务模块目录结构

### 标准模块结构

每个业务模块应遵循以下目录结构：

```
module-name/
├── components/          # 组件目录
│   ├── EditModal.vue
│   ├── DetailDrawer.vue
│   ├── Form.vue
│   └── CardItem.vue
├── types/               # 类型定义目录
│   └── index.ts         # 导出所有类型
├── data.ts              # 数据配置（表格列定义等）
├── mock.ts              # Mock 数据
└── index.vue           # 模块入口页面
```

### 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| 目录 | 全部小写 | `components`, `types` |
| Vue 组件 | PascalCase（帕斯卡命名） | `EditModal.vue`, `DetailDrawer.vue` |
| TypeScript 文件 | 驼峰/帕斯卡 | `data.ts`, `index.ts`, `mock.ts` |
| 类型导出 | 命名导出 | `export interface XxxItem`, `export type XxxMode` |

### 组件组织原则

1. **单一职责**：每个组件只负责一个功能
2. **就近引用**：组件内部只导入同模块的依赖
3. **统一导出**：types/index.ts 导出模块所有类型

### 示例：协议管理模块

```
protocol/
├── components/
│   ├── AuditDetail.vue
│   ├── CategoryForm.vue
│   ├── DeployForm.vue
│   ├── DeviceProtocolModal.vue
│   └── Form.vue
├── types/
│   └── index.ts
├── data.ts
├── mock.ts
└── index.vue
```

### 示例：设备管理模块

```
device/
├── components/
│   ├── DetailDrawer.vue
│   └── EditModal.vue
├── types/
│   └── index.ts
├── data.ts
├── mock.ts
└── index.vue
```

### 类型文件组织

types/index.ts 应包含：

```typescript
// 状态类型
export type XxxStatus = 'active' | 'inactive';

// 选项类型
export type XxxMode = 'create' | 'edit' | 'view';

// 数据接口
export interface XxxItem {
  id: string;
  name: string;
  status: XxxStatus;
}

// 表单数据
export interface XxxFormValues {
  name: string;
  description?: string;
}

// 提交载荷
export interface XxxSubmitPayload {
  id?: string;
  values: XxxFormValues;
}
```

### 导入示例

```typescript
// 从 ./types 导入（自动解析到 index.ts）
import type { XxxItem, XxxStatus } from './types';

// 从 ./components 导入
import EditModal from './components/EditModal.vue';
import DetailDrawer from './components/DetailDrawer.vue';
```

## Mock 数据管理

### mock.ts 最佳实践

将组件中的 Mock 数据分离到 `mock.ts` 文件，保持组件职责单一。

### 正确做法

```typescript
// mock.ts - 独立文件管理所有 Mock 数据
import { markRaw } from 'vue';
import { SvgProductIcon, SvgDeviceIcon } from '@vben/icons';

export const overviewItems = [
  {
    title: 'dashboard.totalProducts',
    value: 1248,
    color: '#3b82f6',
    icon: markRaw(SvgProductIcon),
  },
  // ...
];

export const deviceStatusData = [
  { name: 'dashboard.online', value: 3842, color: '#3b82f6' },
  // ...
];
```

```vue
<!-- index.vue - 只负责展示 -->
<script lang="ts" setup>
import { overviewItems, deviceStatusData } from './mock';
</script>

<template>
  <div v-for="item in overviewItems">{{ item.value }}</div>
</template>
```

### ❌ 错误做法

```vue
<!-- ❌ 不推荐：Mock 数据写在组件内部 -->
<script lang="ts" setup>
const overviewItems = [
  { title: 'dashboard.totalProducts', value: 1248, ... },
  // 200+ 行 Mock 数据
];

const deviceStatusData = [ ... ];
const productCategoryData = [ ... ];
</script>
```

### Mock 数据文件结构

```typescript
// mock.ts
import { markRaw } from 'vue';

// 1. 图标组件（需要 markRaw 防止响应式）
import { SvgIcon1, SvgIcon2 } from '@vben/icons';

// 2. 静态配置数据
export const overviewItems = [ ... ];
export const statusOptions = [ ... ];

// 3. 列表数据
export const alertItems = [ ... ];
export const changeLogItems = [ ... ];

// 4. 图表数据
export const chartData = { ... };
```

### 与 API 集成

后续对接真实 API 时，只需修改 mock.ts：

```typescript
// mock.ts
import { fetchOverview } from '@/api/dashboard';

// 开发环境使用 Mock
export const overviewItems = overviewData;

// 生产环境可改为
// export const overviewItems = await fetchOverview();
```

