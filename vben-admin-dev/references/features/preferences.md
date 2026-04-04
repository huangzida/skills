# 偏好设置配置

项目支持通过 `preferences.ts` 自定义应用行为。

**官方文档**：https://doc.vben.pro/guide/application/preferences.html

## 配置位置

```typescript
// apps/playground/preferences.ts
import { defineOverridesPreferences } from '@vben/preferences';

export const overridesPreferences = defineOverridesPreferences({
  // 配置内容
});
```

## App 配置

| 配置 | 说明 | 默认值 |
|------|------|--------|
| `layout` | 布局方式 | `'sidebar-nav'` |
| `locale` | 语言 | `'zh-CN'` |
| `accessMode` | 权限模式 | `'frontend'` |
| `defaultHomePath` | 默认首页 | `'/'` |
| `watermark` | 水印 | `false` |
| `enableRefreshToken` | 刷新Token | `true` |
| `loginExpiredMode` | 登录过期处理 | `'page'` |
| `authPageLayout` | 登录页布局 | `'panel-right'` |

## Theme 配置

| 配置 | 说明 |
|------|------|
| `mode` | 主题模式：`'light'` \| `'dark'` |
| `builtinType` | 内置主题 |
| `colorPrimary` | 主题色 |
| `colorSuccess` | 成功色 |
| `colorWarning` | 警告色 |
| `colorDestructive` | 错误色 |
| `semiDarkHeader` | 半深色顶栏 |
| `semiDarkSidebar` | 半深色侧边栏 |

## Sidebar 配置

| 配置 | 说明 |
|------|------|
| `collapsed` | 折叠状态 |
| `width` | 侧边栏宽度 |
| `collapsedButton` | 折叠按钮 |
| `fixedButton` | 固定按钮 |

## Tabbar 配置

| 配置 | 说明 |
|------|------|
| `enable` | 启用标签页 |
| `keepAlive` | 缓存页面 |
| `maxCount` | 最大数量 |
| `draggable` | 可拖拽 |

## Widget 配置

| 配置 | 说明 |
|------|------|
| `fullscreen` | 全屏按钮 |
| `refresh` | 刷新按钮 |
| `themeToggle` | 主题切换 |
| `globalSearch` | 全局搜索 |
| `notification` | 通知 |

## 完整示例

```typescript
export const overridesPreferences = defineOverridesPreferences({
  app: {
    layout: 'sidebar-nav',
    locale: 'zh-CN',
    accessMode: 'frontend',
  },
  theme: {
    mode: 'light',
    builtinType: 'default',
    colorPrimary: 'hsl(212 100% 45%)',
  },
  sidebar: {
    collapsed: false,
    width: 224,
  },
  tabbar: {
    enable: true,
    keepAlive: true,
  },
  widget: {
    fullscreen: true,
    refresh: true,
    themeToggle: true,
  },
});
```
