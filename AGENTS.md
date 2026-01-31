# Agent Skills Kits - AGENTS.md

帮助开发者在 Claude Code / Opencode 技能与命令仓库中高效工作。

## 项目概述

本仓库用于开发 **Agent Skills** 和 **Agent Commands**：

- **Skills** (`.opencode/skills/`)：AI 专业能力扩展，通过 `/skill-name` 调用
  - `blog-writer` - 智能博客写作助手
  - `ddd-coding-standards` - DDD 分层架构代码规范
  - `sequence-diagram-generator` - 时序图生成器

- **Commands** (`.opencode/commands/`)：AI 自动执行的命令
  - `intro` - 项目分析与 README 生成
  - `new-command` - 创建新 command
  - `modify-skill` - 修改现有 skill

## 构建与验证

```bash
# 验证所有 SKILL.md 文件
find .opencode/skills -name "SKILL.md" -type f

# 检查 YAML frontmatter
grep -A 5 "^---$" .opencode/skills/*/SKILL.md
```

## 文件规范

### Skill 文件 (SKILL.md)

位置：`.opencode/skills/{skill-name}/SKILL.md`

```yaml
---
name: skill-name                          # kebab-case
description: 一句话描述技能用途          # 必须：帮助 AI 识别场景
argument-hint: "[可选参数]"              # 参数提示
disable-model-invocation: true           # 可选：仅用户可调用
allowed-tools: Read, Glob, Grep          # 可选：限制可用工具
---
```

### Command 文件 (.md)

位置：`.opencode/commands/{command-name}.md`

```yaml
---
description: 命令的一句话描述
argument-hint: "[可选参数]"
---
```

## 目录结构

```
.opencode/
├── skills/
│   └── {skill-name}/
│       ├── SKILL.md          # 必需：技能定义
│       ├── README.md         # 可选：详细文档
│       └── ...               # 可选：脚本、示例等
└── commands/
    └── {command-name}.md     # 命令定义
```

## 工具权限

各 Skill/Command 按需配置 `allowed-tools`：

| 工具 | 用途 |
|------|------|
| Read | 读取示例、模板、用户输入 |
| Write | 保存生成的文件 |
| Glob | 查找目录中的文件 |
| Grep | 搜索代码模式 |
| Bash | 执行脚本 |
| question | 交互式用户输入 |

## Git 工作流

```
<类型>: <描述>
```

**类型：** `feat`（新技能/命令）、`fix`（修复）、`docs`（文档）、`refactor`（重构）

**示例：**
```
feat: 添加 blog-writer 技能
fix: 修正 ddd-coding-standards 依赖规则
```

## 安全指南

1. **无机密**：不包含 API 密钥、密码
2. **相对路径**：避免绝对路径
3. **验证输入**：清理用户输入
4. **最小权限**：限制工具使用范围

## 参考

- [Open Code Commands 文档](https://opencode.ai/docs/commands/)
- [Open Code Skills 文档](https://opencode.ai/docs/skills/)
- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)
- [README.md](./README.md) - 项目完整说明
