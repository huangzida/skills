# 🚀 AI 编码技能集合

[English](README.md) | [中文](README_zh.md)

[![许可协议](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![技能数量](https://img.shields.io/badge/skills-76-green.svg)](#技能总览)
[![最后更新](https://img.shields.io/badge/last%20updated-2026--03--29-orange.svg)](#)

一个全面的AI编码技能集合，旨在增强AI编程助手在软件开发任务中的能力。这些技能旨在与Claude Code、Codex等AI编程代理配合使用。

## 📋 目录

- [概述](#概述)
- [技能总览](#技能总览)
- [快速开始](#快速开始)
- [技能分类](#技能分类)
- [贡献指南](#贡献指南)
- [许可协议](#许可协议)

## 🎯 概述

本仓库包含精选的技能集合，可扩展AI编程助手在各开发领域的能力。每个技能都提供详细的指导、最佳实践和特定任务类别的实现模式。

**核心特性：**
- 📄 **文档处理** - 处理Word、Excel、PDF和PowerPoint文件
- 🎨 **前端开发** - Vue、React、UI/UX、WebGL、前端工具
- 🔧 **代码质量** - 测试、调试、验证、分析
- 🔍 **代码审查** - 代码审查和协作
- 🔒 **安全** - 安全加固和最佳实践
- 🔀 **Git/版本控制** - Git工作流管理
- ⚙️ **DevOps** - CI/CD和自动化
- 🧠 **AI协作** - 头脑风暴、规划、多代理工作流
- 🛠️ **工程工具** - 构建工具、包管理器、框架

## 📊 技能总览

| 分类 | 技能数 | 描述 |
|----------|--------|-------------|
| **文档处理** | 8 | Word、Excel、PDF、PowerPoint处理 |
| **前端开发** | 31 | Vue、React、UI/UX、WebGL、工具 |
| **代码质量** | 12 | 测试、调试、验证、分析 |
| **代码审查** | 3 | 代码审查和协作 |
| **安全** | 1 | 安全加固 |
| **Git/版本控制** | 2 | Git工作流管理 |
| **DevOps** | 1 | CI/CD和自动化 |
| **AI协作** | 9 | 头脑风暴、规划、多代理、技能管理 |
| **工程工具** | 11 | 构建工具、包管理器、框架 |
| **方法论** | 6 | 开发原则和实践 |
| **总计** | **76** | 全面的开发覆盖 |

## 🚀 快速开始

### 安装

使用 skills CLI 安装所有技能：

```bash
npx skills add https://github.com/huangzida/skills
```

安装单个技能：

```bash
npx skills add https://github.com/huangzida/skills --skill <技能名>
# 示例: npx skills add https://github.com/huangzida/skills --skill vben-admin-dev
```

或手动复制到你的AI编程代理技能目录：

```bash
# Claude Code
cp -r skills/* ~/.claude/skills/

# Codex
cp -r skills/* ~/.agents/skills/
```

### 使用方法

技能会根据上下文自动调用。手动调用方式：

```
/skill-name <请求>
```

每个技能都在其`SKILL.md`文件中包含详细文档。

## 📁 技能分类

### 📄 文档处理

| 技能 | 描述 |
|-------|-------------|
| **[pptx](skills/pptx)** | PowerPoint创建、编辑和转换。支持基于模板的编辑和使用pptxgenjs从头生成。 |
| **[pdf](skills/pdf)** | PDF操作，包括读取、合并、分割、OCR、水印和表单填写。 |
| **[xlsx](skills/xlsx)** | Excel文件操作，支持公式、财务建模标准的数据分析。 |
| **[docx](skills/docx)** | Word文档创建和编辑，具有专业格式、表格和修订追踪功能。 |
| **[minimax-docx](skills/minimax-docx)** | MiniMax增强的Word文档处理。 |
| **[minimax-pdf](skills/minimax-pdf)** | MiniMax增强的PDF操作功能。 |
| **[minimax-xlsx](skills/minimax-xlsx)** | MiniMax增强的Excel操作。 |
| **[readme-i18n](skills/readme-i18n)** | README国际化与多语言支持。 |

### 🎨 前端开发

#### Vue

| 技能 | 描述 |
|-------|-------------|
| **[antdv-next](skills/antdv-next)** | Antdv Next Vue 3组件库文档，包含80+组件。 |
| **[vben-admin-dev](skills/vben-admin-dev)** | 基于Vben Admin模式的Vue 3后台开发框架。 |
| **[vue](skills/vue)** | Vue.js核心框架最佳实践。 |
| **[vue-best-practices](skills/vue-best-practices)** | Vue组合式API、响应式、插件开发规范。 |
| **[vue-router-best-practices](skills/vue-router-best-practices)** | Vue Router路由管理最佳实践。 |
| **[vue-testing-best-practices](skills/vue-testing-best-practices)** | Vue组件测试方法论。 |
| **[vueuse-functions](skills/vueuse-functions)** | VueUse组合式函数库参考。 |
| **[pinia](skills/pinia)** | Vue状态管理库Pinia最佳实践。 |

#### React

| 技能 | 描述 |
|-------|-------------|
| **[vercel-react-best-practices](skills/vercel-react-best-practices)** | React和Next.js性能优化，包含8个类别64条规则。 |
| **[vercel-composition-patterns](skills/vercel-composition-patterns)** | React组合模式，用于可扩展的组件架构。 |
| **[shadcn](skills/shadcn)** | shadcn/ui组件库集成和自定义。 |

#### UI/UX

| 技能 | 描述 |
|-------|-------------|
| **[frontend-design](skills/frontend-design)** | 创建独特、生产级的前端界面，具有创意设计。 |
| **[ui-ux-pro-max](skills/ui-ux-pro-max)** | 全面的UI/UX设计智能，包含50+种风格、161种配色方案和99条UX指南。 |
| **[web-design-guidelines](skills/web-design-guidelines)** | 基于Vercel标准的网页界面规范合规性检查。 |
| **[tailwind-design-system](skills/tailwind-design-system)** | Tailwind CSS设计系统构建。 |

#### WebGL

| 技能 | 描述 |
|-------|-------------|
| **[shader-dev](skills/shader-dev)** | GLSL着色器开发，包含36种技术，涵盖光线行进、SDF、流体模拟等。 |

#### 工具

| 技能 | 描述 |
|-------|-------------|
| **[frontend-dev](skills/frontend-dev)** | MiniMax前端开发最佳实践和模式。 |
| **[es-toolkit](skills/es-toolkit)** | 高性能JavaScript实用工具库。200+函数涵盖数组、对象、字符串、Promise等。比lodash体积小97%，速度快2-3倍。 |
| **[adapt](skills/adapt)** | 自适应代码转换和迁移模式。 |
| **[animate](skills/animate)** | 动画技术和实现模式。 |
| **[bolder](skills/bolder)** | 醒目的代码增强和样式模式。 |
| **[colorize](skills/colorize)** | 配色方案和视觉样式工具。 |
| **[delight](skills/delight)** | 令人愉悦的用户体验增强。 |
| **[polish](skills/polish)** | 代码打磨和优化工具。 |
| **[typeset](skills/typeset)** | 代码格式化和排版工具。 |
| **[create-readme](skills/create-readme)** | README文件创建和生成。 |
| **[unocss](skills/unocss)** | UnoCSS原子化CSS引擎配置和预设。 |

### 🛠️ 工程工具

| 技能 | 描述 |
|-------|-------------|
| **[antfu](skills/antfu)** | Anthony Fu开发工作流和工具集。 |
| **[vite](skills/vite)** | Vite下一代前端构建工具配置和插件开发。 |
| **[vitepress](skills/vitepress)** | VitePress静态站点生成器文档编写。 |
| **[vitest](skills/vitest)** | Vite原生单元测试框架。 |
| **[pnpm](skills/pnpm)** | 高性能pnpm包管理器最佳实践。 |
| **[tsdown](skills/tsdown)** | TypeScript打包工具TSdown配置。 |
| **[turborepo](skills/turborepo)** | Turborepo高性能monorepo构建系统。 |

### 🔧 代码质量

#### 测试

| 技能 | 描述 |
|-------|-------------|
| **[test-driven-development](skills/test-driven-development)** | TDD方法论，包含RED-GREEN-REFACTOR循环和压力测试。 |

#### 调试

| 技能 | 描述 |
|-------|-------------|
| **[systematic-debugging](skills/systematic-debugging)** | 系统化调试技术，包括根本原因追踪和条件等待。 |

#### 验证

| 技能 | 描述 |
|-------|-------------|
| **[verification-before-completion](skills/verification-before-completion)** | 完成前的验证清单，确保质量。 |

#### 分析

| 技能 | 描述 |
|-------|-------------|
| **[audit](skills/audit)** | 代码质量审计和评估工具。 |
| **[arrange](skills/arrange)** | 代码整理和组织模式。 |
| **[clarify](skills/clarify)** | 代码清晰度和可读性改进。 |
| **[distill](skills/distill)** | 信息提炼和摘要。 |
| **[extract](skills/extract)** | 数据提取和转换工具。 |
| **[normalize](skills/normalize)** | 代码规范化和标准化。 |
| **[optimize](skills/optimize)** | 性能优化技术。 |
| **[overdrive](skills/overdrive)** | 高级优化和加速。 |
| **[quieter](skills/quieter)** | 错误处理和噪声降低。 |

### 🔍 代码审查

| 技能 | 描述 |
|-------|-------------|
| **[receiving-code-review](skills/receiving-code-review)** | 接收和响应代码审查反馈的指南。 |
| **[requesting-code-review](skills/requesting-code-review)** | 请求有效代码审查的指南。 |
| **[critique](skills/critique)** | 代码批评和反馈模式。 |

### 🔒 安全

| 技能 | 描述 |
|-------|-------------|
| **[harden](skills/harden)** | 安全加固和最佳实践。 |

### 🔀 Git/版本控制

| 技能 | 描述 |
|-------|-------------|
| **[finishing-a-development-branch](skills/finishing-a-development-branch)** | 完成和合并开发分支的工作流程。 |
| **[using-git-worktrees](skills/using-git-worktrees)** | Git worktree管理，用于并行开发。 |

### ⚙️ DevOps

| 技能 | 描述 |
|-------|-------------|
| **[github-actions-docs](skills/github-actions-docs)** | GitHub Actions文档和最佳实践。 |

### 🧠 AI协作

| 技能 | 描述 |
|-------|-------------|
| **[brainstorming](skills/brainstorming)** | 创意设计流程，支持UI/UX项目的可视化配套。 |
| **[executing-plans](skills/executing-plans)** | 带审查检查点的实施计划执行。 |
| **[skillshare](skills/skillshare)** | 从单一来源管理和同步50+工具的AI CLI技能。 |
| **[subagent-driven-development](skills/subagent-driven-development)** | 多代理开发工作流，包含规范审查和代码质量检查。 |
| **[dispatching-parallel-agents](skills/dispatching-parallel-agents)** | 使用独立代理的并行任务执行。 |
| **[find-skills](skills/find-skills)** | 发现和安装可用技能。 |
| **[skill-creator](skills/skill-creator)** | 具有最佳实践的技能创作工具。 |
| **[writing-plans](skills/writing-plans)** | 计划编写和优化方法论。 |
| **[writing-skills](skills/writing-skills)** | 使用TDD方法论创建和测试新技能。 |

### 📚 方法论

| 技能 | 描述 |
|-------|-------------|
| **[pua](skills/pua)** | 方法论参考，包括协议和最佳实践。 |
| **[simple](skills/simple)** | 简化模式和技巧。 |
| **[teach-impeccable](skills/teach-impeccable)** | 教授无可挑剔的编码实践。 |
| **[minimax-multimodal-toolkit](skills/minimax-multimodal-toolkit)** | MiniMax多模态能力工具包。 |
| **[using-superpowers](skills/using-superpowers)** | 技能发现和使用指南。 |
| **[onboard](skills/onboard)** | 开发者入职和设置自动化。 |

## 📈 技能统计

```
文档处理:       █████████████░░░░░░░░░░  8个技能
前端开发:       ██████████████████████████ 31个技能
代码质量:       ███████████████████░░░░░░ 12个技能
代码审查:       █████░░░░░░░░░░░░░░░░░░░░  3个技能
安全:           ██░░░░░░░░░░░░░░░░░░░░░░░  1个技能
Git/版本控制:   ████░░░░░░░░░░░░░░░░░░░░░  2个技能
DevOps:         ██░░░░░░░░░░░░░░░░░░░░░░░  1个技能
AI协作:         ███████████████░░░░░░░░░░░  9个技能
工程工具:       █████████████████░░░░░░░ 11个技能
方法论:         ████████████░░░░░░░░░░░░░  6个技能
───────────────────────────────────────────────────────────
总计:           ████████████████████████ 76个技能
```

## 🔍 技能结构

每个技能都遵循标准化的结构：

```
skill-name/
├── SKILL.md              # 主参考文档（必需）
├── references/           # 详细参考资料
├── techniques/          # 实现技术（复杂技能）
├── rules/               # 特定模式的规则文件
├── scripts/             # 实用脚本
└── examples/            # 使用示例
```

## 🤝 贡献指南

欢迎贡献！请阅读我们的贡献指南：

1. **技能创建** - 遵循[writing-skills](skills/writing-skills)方法论
2. **测试** - 使用基于子代理的测试进行技能验证
3. **文档** - 维护清晰简洁的SKILL.md文件
4. **TDD方法** - 部署前测试技能

详细指南请参见[CONTRIBUTING.md](CONTRIBUTING.md)。

## 📚 其他资源

- [技能编写指南](skills/writing-skills) - 创建新技能
- [技能的TDD](skills/test-driven-development) - 测试方法论
- [头脑风暴流程](skills/brainstorming) - 设计工作流程

## ⚖️ 许可协议

本项目采用MIT许可协议 - 详见[LICENSE](LICENSE)文件。

各技能可能在其各自的`SKILL.md`文件中指定自己的许可协议。

## 🙏 致谢

本集合基于以下最佳实践构建：
- Vercel工程团队
- Anthropic Claude最佳实践
- Anthony Fu (antfu) 开源工具链
- 各类开源项目和方法论

---

**觉得有用就Star一下吧！⭐**
