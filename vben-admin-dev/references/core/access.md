# 权限控制

vben-admin 提供三种权限模式：前端模式、后端模式、混合模式。

**官方文档**：https://doc.vben.pro/guide/essentials/access-control.html

## 权限模式配置

```typescript
// preferences.ts
export const overridesPreferences = defineOverridesPreferences({
  app: {
    accessMode: 'frontend',  // 'frontend' | 'backend' | 'mixed'
  },
});
```

## 前端模式

路由权限在前端固定配置：

```typescript
// 登录时设置用户角色
authStore.setUserInfo({
  ...userInfo,
  roles: ['super', 'admin'],
});
```

## 后端模式

通过接口动态生成路由表：

```typescript
// src/router/access.ts
export async function generateAccess(options) {
  return await generateAccessible(preferences.app.accessMode, {
    fetchMenuListAsync: async () => {
      return await getAllMenusApi();
    },
  });
}
```

## 组件级权限

### AccessControl 组件

```vue
<template>
  <AccessControl :codes="['admin']">
    <Button>仅管理员可见</Button>
  </AccessControl>
</template>
```

### hasAccessByCodes 函数

```vue
<template>
  <Button v-if="hasAccessByCodes(['editor'])">编辑</Button>
</template>

<script setup>
import { hasAccessByCodes } from '@vben/auth';
</script>
```

## 权限码格式

```typescript
// 格式：模块_操作
'user:create'    // 创建用户
'user:update'    // 更新用户
'user:delete'    // 删除用户
'user:read'      // 查看用户
```
