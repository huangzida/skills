# 快速开始

## 环境准备

- Node.js >= 18
- pnpm >= 8
- Git

## 安装依赖

```bash
pnpm install
```

## 启动项目

### Playground（推荐）

```bash
pnpm dev:playground
```

### 其他 UI 框架

```bash
pnpm dev:antd    # Ant Design Vue
pnpm dev:ele     # Element Plus
pnpm dev:naive   # Naive UI
```

## 创建页面

### 1. 添加路由

```typescript
// src/router/routes/modules/xxx.ts
const routes: RouteRecordRaw[] = [
  {
    path: '/example',
    name: 'Example',
    meta: { icon: 'lucide:file-text', title: '示例' },
    children: [
      {
        path: '/example/list',
        name: 'ExampleList',
        component: () => import('#/views/example/list.vue'),
      },
    ],
  },
];
```

### 2. 创建页面组件

```vue
<template>
  <Page title="示例页面">
    <Grid />
  </Page>
</template>
```

### 3. 添加 API

```typescript
// src/api/example.ts
export function getExampleList(params?: any) {
  return requestClient.get('/example/list', { params });
}
```

## 代码检查

```bash
pnpm check:type   # TypeScript 类型检查
pnpm lint         # ESLint 检查
pnpm build        # 构建
```
