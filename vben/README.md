# Vben Admin 5.0 快速开始

## 环境准备

- Node.js >= 18
- pnpm >= 8
- Git

```bash
pnpm install
```

## 启动项目

```bash
pnpm dev:playground   # Playground（推荐新手）
pnpm dev:antd         # Ant Design Vue
pnpm dev:ele          # Element Plus
pnpm dev:naive        # Naive UI
```

## 创建页面流程

### 1. 添加路由

创建 `router/routes/modules/xxx.ts`：

```typescript
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

### 2. 创建页面

```vue
<template>
  <Page title="示例页面">
    <Grid />
  </Page>
</template>
```

### 3. 添加 API

```typescript
export function getExampleList(params?: any) {
  return requestClient.get('/example/list', { params });
}
```

## 常用组件

| 组件 | Hook |
|------|------|
| 表单 | `useVbenForm` |
| 表格 | `useVbenVxeGrid` |
| 弹窗 | `useVbenModal` |
| 抽屉 | `useVbenDrawer` |

## 代码检查

```bash
pnpm check:type   # TypeScript 类型检查
pnpm lint         # ESLint 检查
```

## 下一步

- 查看 [references/components/](references/components/) 学习组件用法
- 查看 [references/guides/crud-generation.md](references/guides/crud-generation.md) 学习 CRUD 开发
- 查看 [references/guides/playground-index.md](references/guides/playground-index.md) 浏览示例代码
