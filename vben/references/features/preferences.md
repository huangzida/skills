# 偏好设置配置

项目支持通过 `preferences.ts` 自定义应用行为。

**官方文档**：https://doc.vben.pro/guide/essentials/settings.html#%E5%81%8F%E5%A5%BD%E8%AE%BE%E7%BD%AE

## 配置位置

```typescript
// apps/xxx/src/preferences.ts
import { defineOverridesPreferences } from '@vben/preferences';

export const overridesPreferences = defineOverridesPreferences({
  // 配置内容
});
```

> **注意**：偏好设置会缓存到浏览器 localStorage，更改配置后需要清空缓存才能生效。

## App 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `accessMode` | 权限模式 | `'frontend'` \| `'backend'` \| `'mixed'` | `'frontend'` |
| `authPageLayout` | 登录页布局 | `'panel-center'` \| `'panel-left'` \| `'panel-right'` | `'panel-right'` |
| `checkUpdatesInterval` | 检查更新轮询时间(分钟) | `number` | `1` |
| `colorGrayMode` | 灰色模式 | `boolean` | `false` |
| `colorWeakMode` | 色弱模式 | `boolean` | `false` |
| `compact` | 紧凑模式 | `boolean` | `false` |
| `contentCompact` | 内容紧凑模式 | `'compact'` \| `'wide'` | `'wide'` |
| `contentCompactWidth` | 内容紧凑宽度 | `number` | `1200` |
| `contentPadding` | 内容内边距 | `number` | `0` |
| `contentPaddingTop` | 内容顶部内边距 | `number` | `0` |
| `contentPaddingBottom` | 内容底部内边距 | `number` | `0` |
| `contentPaddingLeft` | 内容左侧内边距 | `number` | `0` |
| `contentPaddingRight` | 内容右侧内边距 | `number` | `0` |
| `defaultAvatar` | 默认头像 | `string` | 内置默认头像 |
| `defaultHomePath` | 默认首页地址 | `string` | `'/analytics'` |
| `dynamicTitle` | 开启动态标题 | `boolean` | `true` |
| `enableCheckUpdates` | 检查更新 | `boolean` | `true` |
| `enableCopyPreferences` | 显示复制偏好设置按钮 | `boolean` | `true` |
| `enablePreferences` | 显示偏好设置 | `boolean` | `true` |
| `enableRefreshToken` | 刷新Token | `boolean` | `false` |
| `enableStickyPreferencesNavigationBar` | 首选项导航栏吸顶 | `boolean` | `true` |
| `isMobile` | 是否移动端 | `boolean` | `false` |
| `layout` | 布局方式 | 见下方布局类型 | `'sidebar-nav'` |
| `locale` | 语言 | `'zh-CN'` \| `'en-US'` | `'zh-CN'` |
| `loginExpiredMode` | 登录过期处理 | `'page'` \| `'modal'` | `'page'` |
| `name` | 应用名称 | `string` | `'Vben Admin'` |
| `preferencesButtonPosition` | 偏好设置按钮位置 | `'auto'` \| `'fixed'` \| `'header'` | `'auto'` |
| `watermark` | 水印 | `boolean` | `false` |
| `watermarkContent` | 水印文案 | `string` | `''` |
| `zIndex` | z-index | `number` | `200` |

### 布局类型 (LayoutType)

| 值 | 说明 |
|------|------|
| `'sidebar-nav'` | 侧边栏导航 (默认) |
| `'header-nav'` | 顶部导航 |
| `'header-sidebar-nav'` | 顶部+侧边栏导航 |
| `'mixed-nav'` | 混合导航 |
| `'sidebar-mixed-nav'` | 侧边栏混合导航 |
| `'header-mixed-nav'` | 顶部混合导航 |
| `'full-content'` | 全屏内容 |

### 登录页布局类型 (AuthPageLayoutType)

| 值 | 说明 |
|------|------|
| `'panel-center'` | 居中布局 |
| `'panel-left'` | 居左布局 |
| `'panel-right'` | 居右布局 (默认) |

## Theme 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `builtinType` | 内置主题 | 见下方主题类型 | `'default'` |
| `colorDestructive` | 错误色 | `string` (HSL) | `'hsl(348 100% 61%)'` |
| `colorPrimary` | 主题色 | `string` (HSL) | `'hsl(212 100% 45%)'` |
| `colorSuccess` | 成功色 | `string` (HSL) | `'hsl(144 57% 58%)'` |
| `colorWarning` | 警告色 | `string` (HSL) | `'hsl(42 84% 61%)'` |
| `fontSize` | 字体大小 | `number` (px) | `16` |
| `mode` | 主题模式 | `'auto'` \| `'dark'` \| `'light'` | `'dark'` |
| `radius` | 圆角 | `string` | `'0.5'` |
| `semiDarkHeader` | 半深色顶栏 | `boolean` | `false` |
| `semiDarkSidebar` | 半深色侧边栏 | `boolean` | `false` |
| `semiDarkSidebarSub` | 半深色子菜单 | `boolean` | `false` |

### 内置主题类型 (BuiltinThemeType)

`'custom'` \| `'deep-blue'` \| `'deep-green'` \| `'default'` \| `'gray'` \| `'green'` \| `'neutral'` \| `'orange'` \| `'pink'` \| `'red'` \| `'rose'` \| `'sky-blue'` \| `'slate'` \| `'stone'` \| `'violet'` \| `'yellow'` \| `'zinc'`

## Sidebar 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `autoActivateChild` | 点击目录时自动激活子菜单 | `boolean` | `false` |
| `collapsed` | 折叠状态 | `boolean` | `false` |
| `collapsedButton` | 折叠按钮 | `boolean` | `true` |
| `collapsedShowTitle` | 折叠时显示title | `boolean` | `false` |
| `collapseWidth` | 折叠宽度 | `number` | `60` |
| `draggable` | 菜单拖拽 | `boolean` | `true` |
| `enable` | 侧边栏可见 | `boolean` | `true` |
| `expandOnHover` | 悬停自动展开 | `boolean` | `true` |
| `extraCollapse` | 扩展区域折叠 | `boolean` | `false` |
| `extraCollapsedWidth` | 扩展区域折叠宽度 | `number` | `60` |
| `fixedButton` | 固定按钮 | `boolean` | `true` |
| `hidden` | 侧边栏隐藏 | `boolean` | `false` |
| `mixedWidth` | 混合侧边栏宽度 | `number` | `80` |
| `width` | 侧边栏宽度 | `number` | `224` |

## Tabbar 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `draggable` | 可拖拽 | `boolean` | `true` |
| `enable` | 启用标签页 | `boolean` | `true` |
| `height` | 标签页高度 | `number` | `38` |
| `keepAlive` | 缓存页面 | `boolean` | `true` |
| `maxCount` | 最大数量(0为不限) | `number` | `0` |
| `middleClickToClose` | 点击中键关闭标签 | `boolean` | `false` |
| `persist` | 持久化标签 | `boolean` | `true` |
| `showIcon` | 显示图标 | `boolean` | `true` |
| `showMaximize` | 显示最大化按钮 | `boolean` | `true` |
| `showMore` | 显示更多按钮 | `boolean` | `true` |
| `showRefresh` | 显示刷新按钮 | `boolean` | `true` |
| `styleType` | 风格 | `'brisk'` \| `'card'` \| `'chrome'` \| `'plain'` | `'chrome'` |
| `visitHistory` | 访问历史记录 | `boolean` | `true` |
| `wheelable` | 鼠标滚轮响应 | `boolean` | `true` |

## Header 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `enable` | 顶栏可见 | `boolean` | `true` |
| `height` | 顶栏高度 | `number` | `50` |
| `hidden` | 顶栏隐藏(css) | `boolean` | `false` |
| `menuAlign` | 菜单位置 | `'center'` \| `'end'` \| `'start'` | `'start'` |
| `mode` | 显示模式 | `'auto'` \| `'auto-scroll'` \| `'fixed'` \| `'static'` | `'fixed'` |

## Navigation 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `accordion` | 手风琴模式 | `boolean` | `true` |
| `split` | 菜单切割(仅mixed-nav生效) | `boolean` | `true` |
| `styleType` | 菜单风格 | `'plain'` \| `'rounded'` | `'rounded'` |

## Breadcrumb 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `enable` | 面包屑启用 | `boolean` | `true` |
| `hideOnlyOne` | 只有一个时隐藏 | `boolean` | `false` |
| `showHome` | 显示首页图标 | `boolean` | `false` |
| `showIcon` | 显示图标 | `boolean` | `true` |
| `styleType` | 风格 | `'background'` \| `'normal'` | `'normal'` |

## Footer 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `enable` | 底栏可见 | `boolean` | `false` |
| `fixed` | 底栏固定 | `boolean` | `false` |
| `height` | 底栏高度 | `number` | `32` |

## Logo 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `enable` | logo可见 | `boolean` | `true` |
| `fit` | 图片适应方式 | `'contain'` \| `'cover'` \| `'fill'` \| `'none'` \| `'scale-down'` | `'contain'` |
| `source` | logo地址 | `string` | 内置默认logo |
| `sourceDark` | 暗色主题logo | `string` | - |

## Copyright 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `companyName` | 公司名 | `string` | `'Vben'` |
| `companySiteLink` | 公司链接 | `string` | `'https://www.vben.pro'` |
| `date` | 日期 | `string` | `'2024'` |
| `enable` | 版权可见 | `boolean` | `true` |
| `icp` | 备案号 | `string` | `''` |
| `icpLink` | 备案号链接 | `string` | `''` |
| `settingShow` | 设置面板显示 | `boolean` | `true` |

## Transition 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `enable` | 页面切换动画 | `boolean` | `true` |
| `loading` | 页面加载loading | `boolean` | `true` |
| `name` | 动画类型 | `'fade'` \| `'fade-down'` \| `'fade-slide'` \| `'fade-up'` \| `string` | `'fade-slide'` |
| `progress` | 加载进度动画 | `boolean` | `true` |

## ShortcutKeys 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `enable` | 全局快捷键 | `boolean` | `true` |
| `globalLockScreen` | 锁屏快捷键 | `boolean` | `true` |
| `globalLogout` | 注销快捷键 | `boolean` | `true` |
| `globalPreferences` | 偏好设置快捷键 | `boolean` | `true` |
| `globalSearch` | 全局搜索快捷键 | `boolean` | `true` |

## Widget 配置

| 配置 | 说明 | 类型 | 默认值 |
|------|------|------|--------|
| `fullscreen` | 全屏按钮 | `boolean` | `true` |
| `globalSearch` | 全局搜索 | `boolean` | `true` |
| `languageToggle` | 语言切换 | `boolean` | `true` |
| `lockScreen` | 锁屏功能 | `boolean` | `true` |
| `notification` | 通知 | `boolean` | `true` |
| `refresh` | 刷新按钮 | `boolean` | `true` |
| `sidebarToggle` | 侧边栏显示/隐藏 | `boolean` | `true` |
| `themeToggle` | 主题切换 | `boolean` | `true` |
| `timezone` | 时区 | `boolean` | `true` |

> **注意**：`widget` 中没有 `preferences` 配置项。设置按钮的显示由 `app.enablePreferences` 控制。

## 完整示例

```typescript
export const overridesPreferences = defineOverridesPreferences({
  app: {
    name: import.meta.env.VITE_APP_TITLE,
    defaultHomePath: '/dashboard',
    authPageLayout: 'panel-center',
    layout: 'sidebar-nav',
    locale: 'zh-CN',
    accessMode: 'frontend',
  },
  theme: {
    mode: 'dark',
    builtinType: 'default',
    colorPrimary: 'hsl(212 100% 45%)',
    colorSuccess: 'hsl(144 57% 58%)',
    colorWarning: 'hsl(42 84% 61%)',
    colorDestructive: 'hsl(348 100% 61%)',
  },
  sidebar: {
    collapsed: false,
    width: 224,
    collapsedButton: true,
  },
  tabbar: {
    enable: true,
    keepAlive: true,
    styleType: 'chrome',
  },
  header: {
    height: 50,
    mode: 'fixed',
  },
  widget: {
    fullscreen: true,
    refresh: true,
    themeToggle: true,
    globalSearch: true,
    languageToggle: true,
    notification: true,
    timezone: true,
  },
});
```
