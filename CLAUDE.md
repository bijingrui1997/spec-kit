# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Spec Kit 是一个 **Spec-Driven Development (SDD)** 工具，用于生成高质量软件的规范驱动开发流程。该项目包含一个 Python CLI 工具 `specify`，帮助开发者初始化项目、创建规范、生成技术实施计划，并与 AI 编码助手（Claude Code、GitHub Copilot、Gemini CLI）集成。

## Core Commands

### CLI 构建和安装
```bash
# 安装项目（通过 uv）
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>

# 本地开发安装
uv pip install -e .

# 构建项目
python -m build
```

### 核心工作流命令
```bash
# 初始化新项目
specify init <project_name>
specify init --here --ai claude
specify init <project_name> --ai gemini --no-git

# 系统检查
specify check
```

### 项目中的关键命令（初始化后在项目中使用）
```bash
# 创建功能规范
/specify <requirements description>

# 生成技术实施计划
/plan <technical stack and architecture>

# 生成任务列表
/tasks
```

## Architecture

### 项目结构
```
├── src/specify_cli/        # Python CLI 工具源码
├── base/                   # 项目模板基础文件（通过网络获取）
│   ├── memory/            # 项目宪法和更新检查清单
│   ├── scripts/           # Shell 脚本工具
│   ├── templates/         # Markdown 模板文件
│   └── ...
└── pyproject.toml         # Python 项目配置
```

### 核心概念

**Spec-Driven Development 四阶段流程**：
1. **阶段1: 规范创建** (`/specify`) - 描述需求（要什么、为什么）
   - 输入：自然语言需求描述
   - 输出：`specs/###-feature-name/spec.md`
   - 重点：只关注用户需求和业务价值，不涉及技术实现

2. **阶段2: 技术规划** (`/plan`) - 制定技术方案（怎么做）
   - 输入：技术栈和架构选择
   - 输出：`plan.md`, `research.md`, `data-model.md`, `contracts/`, `quickstart.md`
   - 重点：将业务需求转化为技术实施方案

3. **阶段3: 任务分解** (`/tasks`) - 拆解执行步骤（分几步）
   - 输入：第二阶段生成的技术文档
   - 输出：`tasks.md` 包含编号任务清单
   - 重点：按照TDD原则，标记并行任务，确保依赖关系清晰

4. **阶段4: 代码实施** (Implementation) - 编写代码（动手干）
   - 输入：任务清单和技术文档
   - 输出：实际代码文件
   - 重点：严格按照任务清单执行，先写测试再写实现

**模板系统**：
- `spec-template.md` - 功能规范模板
- `plan-template.md` - 技术实施计划模板  
- `tasks-template.md` - 任务列表模板
- `CLAUDE-template.md` - Claude Code 指导模板

### AI 代理集成

该项目设计与以下 AI 编码助手配合使用：
- **Claude Code** - Anthropic 的官方 CLI
- **GitHub Copilot** - GitHub 的 AI 助手
- **Gemini CLI** - Google 的命令行 AI 工具

每个代理都有专门的模板和配置支持。

## Development Workflows

### 规范驱动开发流程

1. **项目初始化**
   - 运行 `specify init` 创建项目结构
   - 选择 AI 代理类型
   - 设置 Git 仓库（可选）

2. **规范创建阶段** (`/specify`)
   - 使用 `/specify` 命令描述需求
   - 专注于 "什么" 和 "为什么"，而非技术细节
   - 自动创建功能分支和 spec.md 文件
   - 通过迭代对话完善规范

3. **技术规划阶段** (`/plan`)
   - 使用 `/plan` 命令指定技术栈
   - 生成详细的实施计划和架构文档
   - 进行必要的技术研究和验证
   - 生成数据模型、API 契约等技术文档

4. **任务分解阶段** (`/tasks`)
   - 分析技术文档，生成具体任务清单
   - 按照 TDD 原则组织任务顺序
   - 标记可并行执行的任务
   - 生成可执行的任务列表

5. **代码实施阶段** (Implementation)
   - AI 代理根据任务清单实施代码
   - 严格按照 TDD：先测试后实现
   - 并行执行独立任务
   - 持续验证和调试

### 模板定制

所有模板都支持组织定制：
- 修改 `templates/` 目录下的模板文件
- 调整 `memory/constitution.md` 中的项目原则
- 更新脚本以符合团队工作流

## Important Concepts

- **Intent-driven Development** - 通过自然语言表达开发意图
- **Specification as Source of Truth** - 规范成为主要产物，代码是其表达
- **Multi-phase Refinement** - 多阶段完善而非一次性生成
- **AI Model Capabilities** - 依赖先进 AI 模型的规范解释能力

## Dependencies

主要 Python 依赖：
- `typer` - CLI 框架
- `rich` - 终端输出美化
- `httpx` - HTTP 客户端
- `platformdirs` - 跨平台目录管理
- `readchar` - 字符输入处理
- `truststore` - SSL/TLS 证书验证

## Prerequisites

- Python 3.11+
- Git
- uv (包管理器)
- AI 编码助手之一：Claude Code、GitHub Copilot 或 Gemini CLI