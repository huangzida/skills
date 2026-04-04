# 状态管理

项目使用 Pinia 进行状态管理。

**官方文档**：https://doc.vben.pro/guide/essentials/state-management.html

## Store 目录结构

```
src/stores/
├── modules/
│   ├── auth.ts
│   └── user.ts
└── index.ts
```

## 创建 Store

```typescript
// src/stores/modules/example.ts
import { defineStore } from 'pinia';
import { ref } from 'vue';

export const useExampleStore = defineStore('example', () => {
  const data = ref<any[]>([]);

  function setData(value: any[]) {
    data.value = value;
  }

  return { data, setData };
});
```

## 使用 Store

```typescript
import { useExampleStore } from '#/stores/modules/example';

const exampleStore = useExampleStore();

// 访问状态
console.log(exampleStore.data);

// 修改状态
exampleStore.setData([...]);
```

## Auth Store

```typescript
import { useAuthStore } from '#/stores/auth';

const authStore = useAuthStore();

// 获取 Token
authStore.accessToken;

// 获取用户信息
authStore.userInfo;

// 设置用户信息
authStore.setUserInfo({ ... });

// 登出
authStore.logout();
```

## 持久化

Store 使用 pinia-plugin-persistedstate 进行持久化存储。
