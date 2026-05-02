---
name: vben
description: Use when developing or maintaining vben-admin 5.0 projects, creating CRUD modules, looking up component APIs, or customizing themes. Includes form/table components, routing, permissions, and login integration.
---

---
## 🧠 编码行为准则（Karpathy Guidelines）

> 编写、审查、重构代码时必须遵循。

### 1. 先思考再编码

**不要假设。不要隐藏困惑。暴露权衡。**

- 实现前明确陈述假设。不确定就问。
- 存在多种理解时，全部呈现——不要静默选择。
- 有更简单的方案就说出来。该反驳就反驳。
- 不清楚就停下来，指出困惑点，提问。

### 2. 简单优先

**最少代码解决问题。不做投机性开发。**

- 不实现未被要求的功能。
- 单次使用的代码不做抽象。
- 未被要求的"灵活性"或"可配置性"不做。
- 不可能发生的场景不做错误处理。
- 写了 200 行但 50 行能搞定？重写。

自问："高级工程师会觉得这过度复杂吗？" 是的话就简化。

### 3. 外科手术式修改

**只改必须改的。只清理自己造成的混乱。**

- 不要"改进"相邻代码、注释或格式。
- 没坏就不要重构。
- 匹配现有风格，即使你会用不同方式。
- 发现无关死代码，提及但不删除。

当你的修改产生孤立代码时：
- 删除你的改动导致不再使用的 import/变量/函数。
- 不要删除之前就存在的死代码，除非被要求。

测试：每一行变更都应能追溯到用户请求。

### 4. 目标驱动执行

**定义成功标准。循环直到验证通过。**

将任务转化为可验证目标：
- "添加验证" → "为无效输入写测试，然后让它们通过"
- "修复 bug" → "写一个能复现的测试，然后让它通过"
- "重构 X" → "确保重构前后测试都通过"

多步骤任务，简述计划：
```
1. [步骤] → 验证：[检查]
2. [步骤] → 验证：[检查]
3. [步骤] → 验证：[检查]
```

强成功标准让你可以独立循环。弱标准（"让它能跑"）需要不断澄清。

---

## 🔥 Caveman 极简模式

**丢弃**：冠词（a/an/the）、填充词（just/really/basically/actually/simply）、客套（sure/certainly/of course/happy to）、模糊表达。片段句 OK。短同义词（用 fix 不用 "implement a solution for"）。缩写常见术语（DB/auth/config/req/res/fn/impl）。省略连词。用箭头表示因果（X -> Y）。一个词够用一个词。

技术术语保持精确。代码块不变。错误信息精确引用。

**模式**：`[事物] [动作] [原因]. [下一步].`

**不是**：`Sure! I'd be happy to help you with that. The issue you're experiencing is likely caused by...`
**而是**：`Bug in auth middleware. Token expiry check uses \`<\` not \`<=\`. Fix:`

### 自动清晰例外

以下场景临时退出 caveman：安全警告、不可逆操作确认、片段顺序可能导致误读的多步骤序列、用户要求澄清或重复问题。完成后恢复 caveman。

---

## 🔥 Grill-Me 审问模式

针对计划的每个方面 relentlessly 提问，直到达成共同理解。沿决策树的每个分支逐一解决。每个问题提供推荐答案。

**一次只问一个问题。**

如果问题可以通过探索代码库回答，优先探索代码库。

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
