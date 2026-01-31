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
  - `new-skill` - 创建新 skill
  - `modify-skill` - 修改现有 skill
  - `git-commit` - 智能 git commit
  - `git-push` - 推送到远程仓库
  - `new-blog` - 快速创建博客
  - `new-sequence-diagram` - 快速创建时序图

## 开发工作流

在开发 Skills 和 Commands 的过程中，使用斜杠命令执行任务时，系统会按照以下优先级查找和执行：

```
1. 优先执行 .opencode/commands/{command-name}.md 中的命令
2. 如果 command 不存在，检查 .opencode/skills/{skill-name}/SKILL.md 中是否有对应 skill
3. 确认是否需要创建新的 skill
```

**工作流示例：**
```bash
# 场景 1：执行已存在的 command
/intro
# → 直接执行 .opencode/commands/intro.md

# 场景 2：执行已存在的 skill
/blog-writer "测试主题"
# → 执行 .opencode/skills/blog-writer/SKILL.md

# 场景 3：创建新 command（不存在时）
/new-command "创建一个代码格式化工具"
# → 提示创建新 command 文件

# 场景 4：创建新 skill（不存在时）
/my-new-skill
# → 确认是否创建新 skill
```

**开发提示：**
- 创建或修改 command/skill 后，立即使用斜杠命令进行测试
- 通过 `/git-commit` 和 `/git-push` 快速提交和推送代码
- 使用 `/new-blog` 和 `/new-sequence-diagram` 快捷执行对应 skill

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
agent: build
description: 命令的一句话描述
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
