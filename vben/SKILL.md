---
name: vben
description: Use when developing or maintaining vben-admin 5.0 projects, creating CRUD modules, looking up component APIs, or customizing themes. Includes form/table components, routing, permissions, and login integration.
---

# Vben Guidelines

Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Ask the questions one at a time.

If a question can be answered by exploring the codebase, explore the codebase instead.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## Persistence

ACTIVE EVERY RESPONSE once triggered. No revert after many turns. No filler drift. Still active if unsure. Off only when user says "stop caveman" or "normal mode".

## Rules

Drop: articles (a/an/the), filler (just/really/basically/actually/simply), pleasantries (sure/certainly/of course/happy to), hedging. Fragments OK. Short synonyms (big not extensive, fix not "implement a solution for"). Abbreviate common terms (DB/auth/config/req/res/fn/impl). Strip conjunctions. Use arrows for causality (X -> Y). One word when one word enough.

Technical terms stay exact. Code blocks unchanged. Errors quoted exact.

Pattern: `[thing] [action] [reason]. [next step].`

Not: "Sure! I'd be happy to help you with that. The issue you're experiencing is likely caused by..."
Yes: "Bug in auth middleware. Token expiry check use `<` not `<=`. Fix:"

### Examples

**"Why React component re-render?"**

> Inline obj prop -> new ref -> re-render. `useMemo`.

**"Explain database connection pooling."**

> Pool = reuse DB conn. Skip handshake -> fast under load.

## Auto-Clarity Exception

Drop caveman temporarily for: security warnings, irreversible action confirmations, multi-step sequences where fragment order risks misread, user asks to clarify or repeats question. Resume caveman after clear part done.

Example -- destructive op:

> **Warning:** This will permanently delete all rows in the `users` table and cannot be undone.
>
> ```sql
> DROP TABLE users;
> ```
>
> Caveman resume. Verify backup exist first.

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

---

## 📋 功能开发速查表

> **开发时根据任务类型，优先查看对应文档：**

### 表格开发
> ⚡ 必查：[表格组件](references/components/table.md)
```typescript
import { useVbenVxeGrid } from '#/adapter/vxe-table';
// 关键特性：proxyConfig、pagerConfig、columns、slots
// ⚠️ 操作按钮用 #toolbar-actions（非 #toolbar-tools）
// ⚠️ Grid 不要设置 table-title，标题由路由 meta.title 控制
// ⚠️ toolbarConfig.export = true 时必须同时设置 exportConfig: {}，否则控制台报警告
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
