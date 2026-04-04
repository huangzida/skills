# 🚀 AI Coding Skills Collection

[English](README.md) | [中文](README_zh.md)

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Skills Count](https://img.shields.io/badge/skills-76-green.svg)](#skills-overview)
[![Last Updated](https://img.shields.io/badge/last%20updated-2026--03--29-orange.svg)](#)

A comprehensive collection of AI coding skills for enhancing AI assistant capabilities in software development tasks. These skills are designed to work with AI coding agents like Claude Code, Codex, and similar tools.

## 📋 Table of Contents

- [Overview](#overview)
- [Skills Overview](#skills-overview)
- [Getting Started](#getting-started)
- [Skills Categories](#skills-categories)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This repository contains a curated collection of skills that extend AI coding assistants' capabilities across various development domains. Each skill provides detailed guidance, best practices, and implementation patterns for specific task categories.

**Key Features:**
- 📄 **Document Processing** - Handle Word, Excel, PDF, PowerPoint files
- 🎨 **Frontend Development** - Vue, React, UI/UX, WebGL, and frontend utilities
- 🔧 **Code Quality** - Testing, debugging, verification, and analysis
- 🔍 **Code Review** - Code review and collaboration
- 🔒 **Security** - Security hardening and best practices
- 🔀 **Git/Version Control** - Git workflow management
- ⚙️ **DevOps** - CI/CD and automation
- 🧠 **AI Collaboration** - Brainstorming, planning, and multi-agent workflows
- 🛠️ **Engineering Tools** - Build tools, package managers, frameworks

## 📊 Skills Overview

| Category | Skills | Description |
|----------|--------|-------------|
| **Document Processing** | 8 | Word, Excel, PDF, PowerPoint handling |
| **Frontend Development** | 31 | Vue, React, UI/UX, WebGL, utilities |
| **Code Quality** | 12 | Testing, debugging, verification, analysis |
| **Code Review** | 3 | Code review and collaboration |
| **Security** | 1 | Security hardening |
| **Git/Version Control** | 2 | Git workflow management |
| **DevOps** | 1 | CI/CD and automation |
| **AI Collaboration** | 9 | Brainstorming, planning, multi-agent, skill management |
| **Engineering Tools** | 11 | Build tools, package managers, frameworks |
| **Methodology** | 6 | Development principles and practices |
| **Total** | **76** | Comprehensive development coverage |

## 🚀 Getting Started

### Installation

Install all skills using the skills CLI:

```bash
npx skills add https://github.com/huangzida/skills
```

Install a single skill:

```bash
npx skills add https://github.com/huangzida/skills --skill <skill-name>
# Example: npx skills add https://github.com/huangzida/skills --skill vben-admin-dev
```

Or manually copy to your AI coding agent's skills directory:

```bash
# For Claude Code
cp -r skills/* ~/.claude/skills/

# For Codex
cp -r skills/* ~/.agents/skills/
```

### Usage

Skills are automatically invoked based on context. For manual invocation:

```
/skill-name <request>
```

Each skill includes detailed documentation in its `SKILL.md` file.

## 📁 Skills Categories

### 📄 Document Processing

| Skill | Description |
|-------|-------------|
| **[pptx](skills/pptx)** | PowerPoint creation, editing, and conversion. Supports template-based editing and from-scratch generation with pptxgenjs. |
| **[pdf](skills/pdf)** | PDF manipulation including reading, merging, splitting, OCR, watermarking, and form filling. |
| **[xlsx](skills/xlsx)** | Excel file operations with formula support, financial modeling standards, and data analysis. |
| **[docx](skills/docx)** | Word document creation and editing with professional formatting, tables, and tracked changes. |
| **[minimax-docx](skills/minimax-docx)** | MiniMax enhanced Word document processing. |
| **[minimax-pdf](skills/minimax-pdf)** | MiniMax enhanced PDF manipulation capabilities. |
| **[minimax-xlsx](skills/minimax-xlsx)** | MiniMax enhanced Excel operations. |
| **[readme-i18n](skills/readme-i18n)** | README internationalization and multilingual support. |

### 🎨 Frontend Development

#### Vue

| Skill | Description |
|-------|-------------|
| **[antdv-next](skills/antdv-next)** | Antdv Next Vue 3 component library documentation with 80+ components. |
| **[vben-admin-dev](skills/vben-admin-dev)** | Vue 3 admin development framework with Vben Admin patterns. |
| **[vue](skills/vue)** | Vue.js core framework best practices. |
| **[vue-best-practices](skills/vue-best-practices)** | Vue Composition API, reactivity, and plugin development standards. |
| **[vue-router-best-practices](skills/vue-router-best-practices)** | Vue Router routing management best practices. |
| **[vue-testing-best-practices](skills/vue-testing-best-practices)** | Vue component testing methodology. |
| **[vueuse-functions](skills/vueuse-functions)** | VueUse composable functions library reference. |
| **[pinia](skills/pinia)** | Pinia state management best practices for Vue. |

#### React

| Skill | Description |
|-------|-------------|
| **[vercel-react-best-practices](skills/vercel-react-best-practices)** | React and Next.js performance optimization with 64 rules across 8 categories. |
| **[vercel-composition-patterns](skills/vercel-composition-patterns)** | React composition patterns for scalable component architecture. |
| **[shadcn](skills/shadcn)** | shadcn/ui component library integration and customization. |

#### UI/UX

| Skill | Description |
|-------|-------------|
| **[frontend-design](skills/frontend-design)** | Creating distinctive, production-grade frontend interfaces with creative design. |
| **[ui-ux-pro-max](skills/ui-ux-pro-max)** | Comprehensive UI/UX design intelligence with 50+ styles, 161 color palettes, and 99 UX guidelines. |
| **[web-design-guidelines](skills/web-design-guidelines)** | Web interface guidelines compliance checking based on Vercel's standards. |
| **[tailwind-design-system](skills/tailwind-design-system)** | Tailwind CSS design system construction. |

#### WebGL

| Skill | Description |
|-------|-------------|
| **[shader-dev](skills/shader-dev)** | GLSL shader development with 36 techniques covering ray marching, SDF, fluid simulation, and more. |

#### Utilities

| Skill | Description |
|-------|-------------|
| **[frontend-dev](skills/frontend-dev)** | MiniMax frontend development best practices and patterns. |
| **[es-toolkit](skills/es-toolkit)** | High-performance JavaScript utility library. 200+ functions for arrays, objects, strings, promises, and more. 97% smaller than lodash, 2-3x faster. |
| **[adapt](skills/adapt)** | Adaptive code transformation and migration patterns. |
| **[animate](skills/animate)** | Animation techniques and implementation patterns. |
| **[bolder](skills/bolder)** | Bold code enhancement and styling patterns. |
| **[colorize](skills/colorize)** | Color scheme and visual styling utilities. |
| **[delight](skills/delight)** | Delightful user experience enhancements. |
| **[polish](skills/polish)** | Code polish and refinement tools. |
| **[typeset](skills/typeset)** | Code formatting and typesetting utilities. |
| **[create-readme](skills/create-readme)** | README file creation and generation. |
| **[unocss](skills/unocss)** | UnoCSS atomic CSS engine configuration and presets. |

### 🛠️ Engineering Tools

| Skill | Description |
|-------|-------------|
| **[antfu](skills/antfu)** | Anthony Fu's development workflow and toolkit collection. |
| **[vite](skills/vite)** | Vite next-generation frontend build tool configuration and plugin development. |
| **[vitepress](skills/vitepress)** | VitePress static site generator for documentation. |
| **[vitest](skills/vitest)** | Vite native unit testing framework. |
| **[pnpm](skills/pnpm)** | High-performance pnpm package manager best practices. |
| **[tsdown](skills/tsdown)** | TypeScript bundler TSdown configuration. |
| **[turborepo](skills/turborepo)** | Turborepo high-performance monorepo build system. |

### 🔧 Code Quality

#### Testing

| Skill | Description |
|-------|-------------|
| **[test-driven-development](skills/test-driven-development)** | TDD methodology with RED-GREEN-REFACTOR cycle and pressure testing. |

#### Debugging

| Skill | Description |
|-------|-------------|
| **[systematic-debugging](skills/systematic-debugging)** | Systematic debugging techniques including root cause tracing and condition-based waiting. |

#### Verification

| Skill | Description |
|-------|-------------|
| **[verification-before-completion](skills/verification-before-completion)** | Pre-completion verification checklist to ensure quality. |

#### Analysis

| Skill | Description |
|-------|-------------|
| **[audit](skills/audit)** | Code quality auditing and assessment tools. |
| **[arrange](skills/arrange)** | Code arrangement and organization patterns. |
| **[clarify](skills/clarify)** | Code clarity and readability improvements. |
| **[distill](skills/distill)** | Information distillation and summarization. |
| **[extract](skills/extract)** | Data extraction and transformation utilities. |
| **[normalize](skills/normalize)** | Code normalization and standardization. |
| **[optimize](skills/optimize)** | Performance optimization techniques. |
| **[overdrive](skills/overdrive)** | Advanced optimization and acceleration. |
| **[quieter](skills/quieter)** | Error handling and noise reduction. |

### 🔍 Code Review

| Skill | Description |
|-------|-------------|
| **[receiving-code-review](skills/receiving-code-review)** | Guidelines for receiving and responding to code review feedback. |
| **[requesting-code-review](skills/requesting-code-review)** | Guidelines for requesting effective code reviews. |
| **[critique](skills/critique)** | Code critique and feedback patterns. |

### 🔒 Security

| Skill | Description |
|-------|-------------|
| **[harden](skills/harden)** | Security hardening and best practices. |

### 🔀 Git/Version Control

| Skill | Description |
|-------|-------------|
| **[finishing-a-development-branch](skills/finishing-a-development-branch)** | Workflow for completing and merging development branches. |
| **[using-git-worktrees](skills/using-git-worktrees)** | Git worktree management for parallel development. |

### ⚙️ DevOps

| Skill | Description |
|-------|-------------|
| **[github-actions-docs](skills/github-actions-docs)** | GitHub Actions documentation and best practices. |

### 🧠 AI Collaboration

| Skill | Description |
|-------|-------------|
| **[brainstorming](skills/brainstorming)** | Creative design process with visual companion support for UI/UX projects. |
| **[executing-plans](skills/executing-plans)** | Implementation plan execution with review checkpoints. |
| **[skillshare](skills/skillshare)** | Manage and sync AI CLI skills across 50+ tools from a single source. |
| **[subagent-driven-development](skills/subagent-driven-development)** | Multi-agent development workflow with spec review and code quality checks. |
| **[dispatching-parallel-agents](skills/dispatching-parallel-agents)** | Parallel task execution with independent agents. |
| **[find-skills](skills/find-skills)** | Discovering and installing available skills. |
| **[skill-creator](skills/skill-creator)** | Skill authoring tool with best practices. |
| **[writing-plans](skills/writing-plans)** | Plan writing and refinement methodology. |
| **[writing-skills](skills/writing-skills)** | Creating and testing new skills with TDD methodology. |

### 📚 Methodology

| Skill | Description |
|-------|-------------|
| **[pua](skills/pua)** | Methodology references including protocols and best practices. |
| **[simple](skills/simple)** | Simplification patterns and techniques. |
| **[teach-impeccable](skills/teach-impeccable)** | Teaching impeccable coding practices. |
| **[minimax-multimodal-toolkit](skills/minimax-multimodal-toolkit)** | MiniMax multimodal capabilities and toolkit. |
| **[using-superpowers](skills/using-superpowers)** | Skill discovery and usage guidance. |
| **[onboard](skills/onboard)** | Developer onboarding and setup automation. |

## 📈 Skill Statistics

```
Document Processing:   █████████████░░░░░░░░░░  8 skills
Frontend Development: ██████████████████████████ 31 skills
Code Quality:         ███████████████████░░░░░░ 12 skills
Code Review:          █████░░░░░░░░░░░░░░░░░░░░  3 skills
Security:             ██░░░░░░░░░░░░░░░░░░░░░░░  1 skill
Git/Version Control:  ████░░░░░░░░░░░░░░░░░░░░░  2 skills
DevOps:               ██░░░░░░░░░░░░░░░░░░░░░░░  1 skill
AI Collaboration:     ███████████████░░░░░░░░░░░  9 skills
Engineering Tools:    █████████████████░░░░░░░░ 11 skills
Methodology:          ████████████░░░░░░░░░░░░░░  6 skills
───────────────────────────────────────────────────────────
Total:               ████████████████████████ 76 skills
```

## 🔍 Skill Structure

Each skill follows a standardized structure:

```
skill-name/
├── SKILL.md              # Main reference document (required)
├── references/           # Detailed reference materials
├── techniques/          # Implementation techniques (for complex skills)
├── rules/               # Rule files for specific patterns
├── scripts/             # Utility scripts
└── examples/            # Usage examples
```

## 🤝 Contributing

Contributions are welcome! Please read our contribution guidelines:

1. **Skill Creation** - Follow the [writing-skills](skills/writing-skills) methodology
2. **Testing** - Use subagent-based testing for skill validation
3. **Documentation** - Maintain clear, concise SKILL.md files
4. **TDD Approach** - Test skills before deploying them

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## 📚 Additional Resources

- [Skills Writing Guide](skills/writing-skills) - Creating new skills
- [TDD for Skills](skills/test-driven-development) - Testing methodology
- [Brainstorming Process](skills/brainstorming) - Design workflow

## ⚖️ License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Individual skills may have their own licenses as specified in their respective `SKILL.md` files.

## 🙏 Acknowledgments

This collection is built upon best practices from:
- Vercel Engineering
- Anthropic Claude Best Practices
- Anthony Fu (antfu) Open Source Tooling
- Various open source projects and methodologies

---

**Star this repo if you find it useful! ⭐**
