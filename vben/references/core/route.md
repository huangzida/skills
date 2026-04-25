# 路由配置

路由配置文件位于 `src/router/routes/` 目录。

**官方文档**：https://doc.vben.pro/guide/essentials/router.html

## 路由文件结构

```
src/router/routes/
├── _core/              # 核心路由（登录、异常页等）
├── index.ts            # 路由汇总
└── modules/            # 业务模块路由
    ├── dashboard.ts
    ├── example.ts
    └── system.ts
```

## 定义路由

```typescript
// src/router/routes/modules/example.ts
import type { RouteRecordRaw } from 'vue-router';

const routes: RouteRecordRaw[] = [
  {
    path: '/example',
    name: 'Example',
    meta: {
      icon: 'lucide:file-text',
      title: '示例页面',
    },
    children: [
      {
        path: '/example/list',
        name: 'ExampleList',
        component: () => import('#/views/example/list.vue'),
        meta: {
          title: '列表页',
        },
      },
    ],
  },
];

export default routes;
```

## 路由元信息

```typescript
meta: {
  icon: 'lucide:home',           // 菜单图标
  title: '页面标题',              // 菜单标题
  order: 1,                      // 菜单排序
  keepAlive: true,               // 是否缓存
  hideBreadcrumb: false,         // 隐藏面包屑
  hideMenu: false,               // 隐藏菜单
  activeMenu: '/dashboard',      // 高亮的菜单
  noBasicLayout: false,         // 不使用基础布局
}
```

## 外链菜单

```typescript
{
  path: '/external',
  name: 'External',
  meta: {
    title: '外部链接',
    href: 'https://example.com',
    target: '_blank',
  },
}
```

## 动态路由

后端返回路由时使用：

```typescript
// src/router/access.ts
export async function accessGuard() {
  // 后端模式下动态添加路由
  const menuList = await getMenuListApi();
  // 动态注册路由...
}
```
