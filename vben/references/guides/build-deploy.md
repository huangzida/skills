# 构建与部署

## 构建命令

```bash
# 构建所有应用
pnpm build

# 构建单个应用
pnpm build:playground
pnpm build:antd
pnpm build:naive
pnpm build:ele
```

## 构建配置

### 环境变量

```bash
# .env.production
VITE_API_URL=https://api.example.com
```

### 部署配置

```typescript
// vite.config.ts
export default defineConfig({
  base: '/admin/',  // 部署路径
});
```

## 部署

### Nginx 配置

```nginx
location /admin/ {
  alias /var/www/admin/;
  try_files $uri $uri/ /admin/index.html;
}
```

### Docker 部署

```dockerfile
FROM nginx:alpine
COPY dist/ /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/conf.d/default.conf
```
