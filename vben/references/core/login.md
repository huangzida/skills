# 登录对接

对接自定义后端登录接口的完整指南。

**官方文档**：https://doc.vben.pro/guide/application/login.html

## 需要实现的接口

### 登录接口

```typescript
export async function loginApi(data: LoginParams) {
  return requestClient.post<{ accessToken: string }>('/auth/login', data);
}
```

### 获取用户信息接口

```typescript
export async function getUserInfoApi() {
  return requestClient.get<UserInfo>('/user/info');
}
```

### 刷新 Token 接口（可选）

```typescript
export async function refreshTokenApi() {
  return requestClient.post<{ accessToken: string }>('/auth/refresh');
}
```

## 权限模式配置

### 前端模式

```typescript
export const overridesPreferences = defineOverridesPreferences({
  app: {
    accessMode: 'frontend',
  },
});
```

### 后端模式

```typescript
export const overridesPreferences = defineOverridesPreferences({
  app: {
    accessMode: 'backend',
  },
});
```

## 登录过期处理

```typescript
export const overridesPreferences = defineOverridesPreferences({
  app: {
    loginExpiredMode: 'page',  // 'page' | 'modal'
  },
});
```

## Token 刷新

```typescript
export const overridesPreferences = defineOverridesPreferences({
  app: {
    enableRefreshToken: true,
  },
});
```

## 登录页布局

```typescript
export const overridesPreferences = defineOverridesPreferences({
  app: {
    authPageLayout: 'panel-right',  // 'panel-left' | 'panel-right' | 'panel-top'
  },
});
```
