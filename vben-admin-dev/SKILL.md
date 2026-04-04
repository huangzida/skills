---
name: vben-admin-dev
description: Use when developing or maintaining vben-admin 5.0 projects, creating CRUD modules, looking up component APIs, or customizing themes. Includes form/table components, routing, permissions, and login integration.
---

# Vben Admin 5.0 开发指南

面向 vben-admin 5.0 项目的高级开发指南，整合官方文档和最佳实践。

## ⚠️ 开发规范

> **组件超过 200 行或包含多个功能区块时必须拆分！**

保持组件颗粒度细、易于维护和扩展：
- 将大组件拆分为多个小组件
- 每个组件职责单一
- 相关功能抽离为独立组件

| 场景 | 拆分方式 |
|------|----------|
| 表单 + 弹窗 | 使用 `useVbenForm` + `useVbenModal` |
| 复杂表单 | 按业务模块拆分为多个子表单 |
| 编辑详情 | 基本信息 + 物模型 等模块拆分 |
| 仪表板 | Header + 卡片组 + 图表 + 列表 等区块拆分 |

> 💡 **详细指南：** [组件拆分最佳实践](references/guides/component-splitting.md)

## 快速导航

### 🔰 入门

| 主题 | 文件 |
|------|------|
| [快速开始](references/guides/quick-start.md) | 环境准备、启动项目、创建页面 |
| [项目结构](references/guides/project-structure.md) | Monorepo 结构、路径别名 |
| [Playground 示例](references/guides/playground-index.md) | 表单、表格、弹窗示例索引 |
| [组件拆分](references/guides/component-splitting.md) | 仪表板/卡片等组件拆分最佳实践 |

### 📦 核心组件

| 组件 | 文件 | Playground |
|------|------|-----------|
| [表单](references/components/form.md) | useVbenForm | `examples/form/` |
| [表格](references/components/table.md) | useVbenVxeGrid | `examples/vxe-table/` |
| [图表](references/components/echarts.md) | EchartsUI + useEcharts | `dashboard/analytics/` |
| [弹窗/抽屉](references/components/modal-drawer.md) | useVbenModal/Drawer | `examples/modal/` |
| [页面](references/components/page.md) | Page | `examples/page/` |

### 🏗️ 核心功能

| 功能 | 文件 |
|------|------|
| [路由配置](references/core/route.md) | 路由定义、元信息、动态路由 |
| [权限控制](references/core/access.md) | 前端/后端模式、组件级权限 |
| [API请求](references/core/api.md) | 请求封装、拦截器 |
| [国际化](references/core/internationalization.md) | 语言包、切换语言 |
| [状态管理](references/core/state-management.md) | Store、Auth Store |
| [主题配置](references/core/theme.md) | 内置主题、自定义主题 |
| [样式设计](references/core/styling.md) | Tailwind 主题类、深色模式、语义颜色 |
| [图标使用](references/core/icons.md) | Iconify、常用图标集 |
| [登录对接](references/core/login.md) | 登录接口、Token刷新 |
| [适配器](references/core/adapter.md) | 组件适配、自定义注册 |

### ⚙️ 配置与定制

| 主题 | 文件 |
|------|------|
| [偏好设置](references/features/preferences.md) | preferences.ts 完整配置 |
| [CRUD生成](references/guides/crud-generation.md) | 标准CRUD开发流程 |
| [构建部署](references/guides/build-deploy.md) | 构建命令、部署配置 |

## 常用命令

```bash
# 开发
pnpm dev:playground   # Playground（推荐）

# 检查
pnpm check:type       # 类型检查
pnpm lint             # ESLint

# 构建
pnpm build            # 构建所有
```

## 官方资源

- [官方文档](https://doc.vben.pro)
- [GitHub](https://github.com/vbenjs/vue-vben-admin)
- [Iconify 图标](https://icon-sets.iconify.design/)
- [ECharts](https://echarts.apache.org/zh/index.html)

## 目录结构

```
vben-admin-dev/
├── SKILL.md                    # 主入口
├── README.md                   # 快速入门
├── references/
│   ├── components/             # 组件文档
│   ├── core/                   # 核心功能
│   ├── features/               # 配置定制
│   └── guides/                 # 开发指南
│       └── component-splitting.md  # 组件拆分最佳实践
```
