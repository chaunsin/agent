# Agent Skills

[English](README.md)

AI agent 技能集合 — 可被 AI 编程助手（如 Claude Code、Copilot CLI、Cursor）加载的独立知识库，用于在特定领域提供专业级辅助。

## 什么是 Skill？

Skill 是带有 YAML frontmatter 的结构化 Markdown 文档，作为特定主题的综合参考。AI agent 加载 skill 后可以在无需实时搜索文档的情况下，提供更准确、更具上下文感知的协助。

每个 skill 位于 `skills/<name>/` 目录下，包含：

- **`SKILL.md`** — 入口文件，包含 frontmatter 元数据和快速参考指南
- **`references/`** — 模块化的深入文件，涵盖详细语法、边缘情况和工作流

## 可用 Skills

| Skill    | 说明                                                                   |
| -------- | ---------------------------------------------------------------------- |
| [psql][] | PostgreSQL 交互式终端参考 — 元命令、CLI 选项、格式化、数据导入/导出、脚本编写及高级工作流 |

[psql]: skills/psql/SKILL.md

## 安装

### npx skills（推荐）

[skills CLI](https://github.com/vercel-labs/skills) 是一个通用安装工具，支持 Claude Code、Cursor、Copilot、Codex 等 40 多种 agent。

```bash
# 安装本仓库所有 skills
npx skills add chaunsin/agent

# 安装指定 skill
npx skills add chaunsin/agent --skill psql

# 安装前先查看可用 skills
npx skills add chaunsin/agent --list

# 全局安装（所有项目可用）
npx skills add chaunsin/agent -g

# 安装到指定 agent
npx skills add chaunsin/agent -a claude-code -a cursor
```

### 手动安装

```bash
# 复制 skill 到 Claude Code skills 目录
cp -r skills/<skill-name> ~/.claude/skills/
```

## 使用方法

安装后，skill 会根据其触发关键词（定义在 `description` frontmatter 字段中）自动激活，无需手动配置。当你提出与 skill 领域相关的问题时，agent 会自动加载相应参考。

例如，安装 psql skill 后，询问 "how to list all tables in psql" 或 "psql \d 命令" 即会触发该 skill。

## 仓库结构

```text
agent/
├── skills/                  # Skill 目录
│   └── psql/                # PostgreSQL 交互式终端
│       ├── SKILL.md         # Skill 入口文件
│       └── references/      # 详细参考文件
├── CLAUDE.md                # Claude Code 工作区指令
├── LICENSE                  # Apache 2.0 许可证
└── README.md                # 英文说明文件
```

## 许可证

[Apache License 2.0](LICENSE)
