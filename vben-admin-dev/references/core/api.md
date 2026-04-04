# API 请求

请求配置位于 `src/api/request.ts`，基于 axios 封装。

**官方文档**：https://doc.vben.pro/guide/essentials/server.html

## 请求封装

```typescript
// src/api/request.ts
import { createRequestClient } from '#/api/client';

export const requestClient = createRequestClient({
  // 请求基础配置
});
```

## 定义 API

```typescript
// src/api/example.ts
import { requestClient } from '#/api/request';

export function getExampleList(params?: any) {
  return requestClient.get('/example/list', { params });
}

export function createExample(data: any) {
  return requestClient.post('/example', data);
}

export function updateExample(id: number, data: any) {
  return requestClient.put(`/example/${id}`, data);
}

export function deleteExample(id: number) {
  return requestClient.delete(`/example/${id}`);
}
```

## 请求配置

| 配置 | 说明 |
|------|------|
| `timeout` | 请求超时时间 |
| `apiUrl` | API 基础地址 |
| `errorMessageMode` | 错误提示模式 |

## 拦截器

在 `createRequestClient` 中配置：

```typescript
// 请求拦截器
requestClient.interceptors.request.use((config) => {
  // 添加 Token
  const token = useAccessStore().accessToken;
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// 响应拦截器
requestClient.interceptors.response.use((response) => {
  return response.data;
}, (error) => {
  // 统一错误处理
  return Promise.reject(error);
});
```
