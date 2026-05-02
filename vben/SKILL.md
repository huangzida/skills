---
name: vben
description: Use when developing or maintaining vben-admin 5.0 projects, creating CRUD modules, looking up component APIs, or customizing themes. Includes form/table components, routing, permissions, and login integration.
---

# Vben Admin 5.0 开发指南

面向 vben-admin 5.0 项目的高级开发指南，整合官方文档和最佳实践。

---

## 🚀 如何使用本 Skill

**每次开发任务时：**
1. 先阅读 [强制开发规范](references/rules.md) 的快速检查清单
2. 根据任务类型查看对应的「功能开发速查表」
3. 实现时遵循参考文档的最佳实践
4. 开发完成后运行 `pnpm lint && pnpm check:type`

---

## ⚠️ 强制开发规范（必读）

> **🔴 开发任何任务前，必须先阅读 [强制开发规范](references/rules.md)！**
>
> 违反这些规范将导致 lint 错误或运行时异常。

**快速检查清单：**
- [ ] 使用 `useVbenVxeGrid`（非 antdv Table）
- [ ] 使用 `useVbenModal`（非 antdv Modal）
- [ ] 使用 `useVbenForm`（非原生表单）
- [ ] 组件 >200行 或 ≥2个功能区块 → 必须拆分
- [ ] 禁止非空断言 `!`，用 `??` / `?.`
- [ ] 深拷贝用 `structuredClone()`
- [ ] 图标用 `@vben/icons`
- [ ] `gridOptions.height` 不设置 `'auto'`
- [ ] 每个页面独立一个国际化 JSON 文件

---

## 📋 功能开发速查表

> **开发时根据任务类型，优先查看对应文档：**

### 表格开发
> ⚡ 必查：[表格组件](references/components/table.md)
```typescript
import { useVbenVxeGrid } from '#/adapter/vxe-table';
// 关键特性：proxyConfig、pagerConfig、columns、slots
```

### 表单开发
> ⚡ 必查：[表单组件](references/components/form.md)
```typescript
import { useVbenForm } from '#/adapter/form';
// 关键特性：componentProps、valueFormat、fieldMappingTime
```

### 弹窗/抽屉
> ⚡ 必查：[弹窗抽屉](references/components/modal-drawer.md)
```typescript
import { useVbenModal } from '@vben/common-ui';
// 关键特性：connectedComponent、appendToMain
```

### 仪表板/统计卡片
> ⚡ 必查：[组件拆分](references/guides/component-splitting.md)
```typescript
// 仪表板拆分模式：
// - StatCardList     统计卡片
// - ChartCard/Chart  图表组件
// - TableList        数据列表
// - index.vue        主组件（布局协调）
```

### ECharts 图表
> ⚡ 必查：[图表组件](references/components/echarts.md)
```typescript
import { EchartsUI, useEcharts } from '@vben/plugins/echarts';
// 关键特性：useEcharts、renderEcharts
```

### 文件下载
> ⚡ 必查：[文件下载](references/features/download.md)
```typescript
requestClient.download(url, filename);
```

### 图标使用
> ⚡ 必查：[图标使用](references/core/icons.md)
```typescript
import { Plus, Settings } from '@vben/icons';
// ⚠️ 禁止内联 SVG！
```

### 路由配置
> ⚡ 必查：[路由配置](references/core/route.md)
```typescript
meta: {
  icon: 'lucide:file-text',
  title: '页面标题',
  order: 1,
}
```

---

## 📁 完整文档索引

### 核心功能

| 功能 | 文档 | 关键特性 |
|------|------|----------|
| [表格](references/components/table.md) | table.md | proxyConfig、pagerConfig、columns |
| [表单](references/components/form.md) | form.md | componentProps、valueFormat |
| [弹窗抽屉](references/components/modal-drawer.md) | modal-drawer.md | connectedComponent、appendToMain |
| [图表](references/components/echarts.md) | echarts.md | useEcharts、renderEcharts |
| [页面](references/components/page.md) | page.md | Page 组件 |
| [下拉菜单](references/components/dropdown-popconfirm.md) | dropdown-popconfirm.md | Popconfirm 事件处理 |
| [数字动画](references/components/count-to-animator.md) | count-to-animator.md | 数字滚动动画 |
| [路由配置](references/core/route.md) | route.md | children、meta、order |
| [权限控制](references/core/access.md) | access.md | permission |
| [API请求](references/core/api.md) | api.md | requestClient |
| [国际化](references/core/internationalization.md) | internationalization.md | $t |
| [主题样式](references/core/styling.md) | styling.md | Tailwind、dark mode |
| [图标使用](references/core/icons.md) | icons.md | @vben/icons |
| [状态管理](references/core/state-management.md) | state-management.md | Pinia Store |
| [适配器](references/core/adapter.md) | adapter.md | 自定义组件 |
| [登录对接](references/core/login.md) | login.md | 登录接口、Token刷新 |

### 功能配置

| 功能 | 文档 |
|------|------|
| [文件下载](references/features/download.md) | requestClient.download() |
| [偏好设置](references/features/preferences.md) | preferences.ts |
| [CRUD生成](references/guides/crud-generation.md) | 标准 CRUD 流程 |

### 开发指南

| 指南 | 文档 | 说明 |
|------|------|------|
| [组件拆分](references/guides/component-splitting.md) | component-splitting.md | ⚠️ 强制规范 |
| [快速开始](references/guides/quick-start.md) | quick-start.md | 环境、项目结构 |
| [Playground](references/guides/playground-index.md) | playground-index.md | 示例索引 |
| [构建部署](references/guides/build-deploy.md) | build-deploy.md | 构建命令 |
| [项目结构](references/guides/project-structure.md) | project-structure.md | 目录规范 |

---

## 🔧 开发流程

```
1. 任务分析 → 确定功能模块类型（表格/表单/仪表板/图表）
2. 规范检查 → 阅读 references/rules.md 强制规范
3. 组件选择 → 根据「功能开发速查表」查看对应文档
4. 组件拆分 → 仪表板/复杂组件参考 component-splitting.md
5. 实现开发 → 遵循组件文档的最佳实践
6. 验证检查 → pnpm lint && pnpm check:type
```

---

## 📝 常用命令

```bash
# 开发
pnpm dev:playground   # Playground（推荐）

# 检查
pnpm lint             # Lint 检查
pnpm check:type       # 类型检查

# 构建
pnpm build            # 构建所有
```

---

## 🔗 官方资源

- [官方文档](https://doc.vben.pro)
- [GitHub](https://github.com/vbenjs/vue-vben-admin)
- [Iconify 图标](https://icon-sets.iconify.design/)
- [ECharts](https://echarts.apache.org/zh/index.html)

---

## 📂 目录结构

```
vben/
├── SKILL.md                    # 主入口
├── README.md                   # 快速入门
└── references/
    ├── rules.md                # ⚠️ 强制开发规范（必读）
    ├── components/             # 组件文档
    │   ├── table.md           # 表格
    │   ├── form.md            # 表单
    │   ├── modal-drawer.md    # 弹窗
    │   ├── echarts.md         # 图表
    │   ├── page.md            # 页面
    │   ├── dropdown-popconfirm.md
    │   └── count-to-animator.md
    ├── core/                   # 核心功能
    │   ├── route.md           # 路由
    │   ├── access.md          # 权限
    │   ├── api.md             # API
    │   ├── internationalization.md
    │   ├── styling.md         # 主题
    │   ├── icons.md           # 图标
    │   ├── state-management.md
    │   ├── adapter.md
    │   └── login.md
    ├── features/              # 功能配置
    │   ├── download.md
    │   └── preferences.md
    └── guides/                 # 开发指南
        ├── component-splitting.md  # ⚠️ 组件拆分（强制规范）
        ├── quick-start.md
        ├── playground-index.md
        ├── build-deploy.md
        └── project-structure.md
```
