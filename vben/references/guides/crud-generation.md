# CRUD 模块生成流程

完整的 CRUD 模块开发流程。

## 执行流程

### 1. 需求收集

收集以下信息：
- 模块名称（英文）
- 字段列表（名称、类型、是否必填）
- 特殊功能需求

### 2. 文件结构

```
src/
├── api/
│   └── [module]/
│       ├── types.ts     # 类型定义
│       └── index.ts     # API 接口
├── router/
│   └── routes/
│       └── modules/
│           └── [module].ts   # 路由配置
└── views/
    └── [module]/
        ├── index.vue        # 主页面
        └── modules/
            └── form.vue     # 表单组件
```

### 3. 类型定义

```typescript
// src/api/[module]/types.ts
export interface [Module]Entity {
  id: number;
  name: string;
  status: number;
}

export interface [Module]ListParams {
  page: number;
  pageSize: number;
}
```

### 4. API 接口

```typescript
// src/api/[module]/index.ts
import { requestClient } from '#/api/request';

export function get[Module]List(params: any) {
  return requestClient.get('/[module]/list', { params });
}

export function create[Module](data: any) {
  return requestClient.post('/[module]', data);
}

export function update[Module](id: number, data: any) {
  return requestClient.put(`/[module]/${id}`, data);
}

export function delete[Module](id: number) {
  return requestClient.delete(`/[module]/${id}`);
}
```

### 5. 路由配置

```typescript
// src/router/routes/modules/[module].ts
const routes: RouteRecordRaw[] = [
  {
    path: '/[module]',
    name: '[Module]',
    meta: { icon: 'lucide:folder', title: '[模块名]' },
    children: [
      {
        path: '/[module]/list',
        name: '[Module]List',
        component: () => import('#/views/[module]/index.vue'),
      },
    ],
  },
];
```

### 6. 主页面

```vue
<script setup lang="ts">
import { Page } from '@vben/common-ui';
import { useVbenDrawer } from '@vben/common-ui';
import { useVbenVxeGrid } from '#/adapter/vxe-table';
import { get[Module]List, delete[Module] } from '#/api/[module]';

const [Grid] = useVbenVxeGrid({
  gridOptions: {
    columns: [...],
    proxyConfig: { ajax: { query: get[Module]List } },
  },
});
</script>

<template>
  <Page auto-content-height>
    <Grid />
  </Page>
</template>
```

## 代码质量检查

```bash
pnpm check:type   # 类型检查
pnpm lint         # ESLint 检查
pnpm build        # 构建检查
```

## Playground 参考

| 示例 | 路径 |
|------|------|
| 基础表单 | `examples/form/basic.vue` |
| 表格CRUD | `examples/vxe-table/` |
