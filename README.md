# Agent Skills Kits

> 一组精心设计的 Claude Code / Opencode **Skills（技能）**与 **Commands（命令）**，扩展 AI 助手在博客写作、代码规范、项目管理等领域的专业能力。

## 简介

Agent Skills Kits 是一个开源的 AI 能力扩展集合，包含两类组件：

- **Skills（技能）**：AI 专业能力扩展，自动识别场景并介入，通过 `/skill-name` 调用
- **Commands（命令）**：AI 自动执行的特定任务，通过 `/command-name` 调用

所有组件均为独立的、自包含的模块，通过 YAML frontmatter 和 Markdown 定义，即插即用。

**核心价值：**
- 🚀 **即装即用** - 复制到 `.opencode/skills/` 或 `.opencode/commands/` 即可使用
- 📝 **中文优先** - 所有组件针对中文用户场景设计
- 🎯 **专业聚焦** - 每个组件专注于特定领域的专业能力
- 🛠️ **可扩展** - 基于标准规范，易于创建和定制新组件

## 快速开始

```bash
# 安装 Skills
mkdir -p ~/.opencode/skills
cp -r .opencode/skills/* ~/.opencode/skills/

# 安装 Commands
mkdir -p ~/.opencode/commands
cp -r .opencode/commands/* ~/.opencode/commands/

# 使用示例
/blog-writer "如何学习投资" --type=经验分享 --style=朴实稳重
/intro
/git-commit
```

## 可用 Skills

| 命令 | 描述 | 参数 |
|------|------|------|
| `/blog-writer` | 智能博客写作助手 | `[文章主题] --type=[类型] --style=[风格] --words=[字数] --input=[已有内容文件]` |
| `/ddd-coding-standards` | DDD 分层架构代码规范检查 | `[任意问题]` |
| `/sequence-diagram-generator` | 将 Mermaid 时序图转换为 PNG/SVG 图片 | `[时序图代码]` |

## 可用 Commands

| 命令 | 描述 | 参数 |
|------|------|------|
| `/intro` | 创建或更新项目的 README.md 来介绍项目 | - |
| `/new-command` | 创建一个新的 Agent command | `[功能描述]` |
| `/new-skill` | 创建新的 Agent skill | `[功能描述]` |
| `/modify-skill` | 修改已有的 Agent Skill | `[skill-name]` |
| `/git-commit` | 执行 git commit 来提交当前工作区的变更 | `[提交信息提示]` |
| `/git-push` | 执行 git push 来向远程仓库推送本地的 commit | `[目标分支名]` |
| `/new-blog` | 执行 Agent Skill：blog-writer | - |
| `/new-sequence-diagram` | 执行 Agent Skill：sequence-diagram-generator | - |

## 组件详情

### 1. Blog Writer - 智能博客写作助手

专业博客文章写作助手，支持交互式配置，智能学习示例风格。

**核心功能：**
- 🎨 **5 大主题**：运动健康、投资理财、生活杂谈、软件开发、脱口秀
- 📄 **7 种类型**：技术分享、产品分析、观点评论、学习笔记、工具推荐、经验分享、成长故事
- ✍️ **6 种风格**：故事叙述、朴实稳重、轻松幽默、简洁明了、专业严谨、浮夸搞笑
- 💾 **自动归档**：生成文章保存到 `archives/` 目录
- 📖 **智能学习**：从示例文章学习写作风格
- 🔄 **素材扩展**：支持基于已有内容进行扩展写作

### 2. DDD Coding Standards - DDD 分层架构代码规范

Java + SpringBoot + Gradle 项目的 DDD 分层架构代码规范专家。

**核心规范：**
- 🏗️ **标准四层架构**：接口层、应用层、领域层、基础设施层
- 📐 **依赖方向控制**：严格的层间依赖规则
- 🎯 **领域层独立性**：核心逻辑完全独立，不依赖框架
- 📝 **命名规范**：杜绝模糊命名，代码自解释
- 🧪 **测试规范**：JUnit 5、MockBean 正确使用规范
- 📋 **检查清单**：提供完整的规范检查清单

### 3. Sequence Diagram Generator - 时序图生成器

将 Mermaid 时序图代码转换为 PNG 图片，无需本地依赖。

**核心特性：**
- 🌐 **在线渲染**：使用 Kroki.io 免费服务
- 🖼️ **PNG/SVG 输出**：高质量图片
- 📁 **自定义目录**：支持指定保存位置
- 📜 **无需依赖**：无需本地安装 Mermaid

### 4. Intro - 项目分析与 README 生成

智能分析项目结构，识别项目类型，自动生成或更新专业的 README.md。

**核心功能：**
- 🔍 **项目扫描**：自动识别项目类型和核心文件
- 📝 **智能生成**：根据项目特征生成 README 结构
- 🔄 **增量更新**：现有 README 智能合并补充
- 🎯 **多类型支持**：支持代码项目、文档库、工具集合等多种类型

### 5. New Command - 创建新 Command

根据功能描述自动提炼命令名称，在 `.opencode/commands/` 目录生成 Command 文件。

**工作流程：**
1. 收集功能描述
2. 自动提炼命令名称（kebab-case）
3. 生成完整的命令定义文件
4. 包含执行流程、工具推荐、示例用法

### 6. New Skill - 创建新 Skill

根据功能描述自动创建新的 Skill，包含 SKILL.md 和目录结构。

**工作流程：**
1. 收集功能描述
2. 自动提炼技能名称和描述
3. 生成 SKILL.md 文件
4. 创建标准目录结构

### 7. Modify Skill - 修改现有 Skill

选择并修改现有的 Skill，支持智能更新 SKILL.md 文件。

**工作流程：**
1. 列出可用 Skills 供选择
2. 收集修改描述
3. 读取并分析现有 Skill 文件
4. 智能执行修改（支持多种修改类型）
5. 确认修改并保存（自动备份）

### 8. Git Commit - 智能 Git 提交

分析当前工作区代码变更，智能生成符合 Conventional Commits 规范的英文提交信息，并帮用户执行 git commit。

**核心功能：**
- 📊 **变更分析**：自动统计新增、修改、删除的文件
- 🎯 **类型识别**：智能识别 feat/fix/docs/refactor/chore 等类型
- 📝 **信息生成**：生成规范的提交信息（可选用户提供提示）
- ✅ **用户确认**：执行前展示完整的变更统计和提交信息预览

### 9. Git Push - 推送到远程仓库

帮助用户将本地的 git commits push 到远程的 git 仓库，确保只推送当前分支的 commits。

**核心功能：**
- 📋 **Commit 预览**：展示所有待推送的 commits 和变更统计
- 🎯 **分支选择**：支持交互式选择目标分支
- ⚠️ **安全确认**：推送前展示完整推送命令预览
- 🔄 **错误处理**：提供常见错误的解决方案建议

### 10. New Blog - 快速创建博客

立即执行 `blog-writer` skill 来帮助用户创建博客，提供便捷的快速入口。

### 11. New Sequence Diagram - 快速创建时序图

立即执行 `sequence-diagram-generator` skill 来创建时序图，提供便捷的快速入口。

## 项目结构

```
agent-skills-kits/
├── .opencode/
│   ├── skills/                    # Skills 目录
│   │   ├── blog-writer/           # 博客写作助手
│   │   │   ├── SKILL.md           # 技能定义
│   │   │   ├── README.md          # 详细文档
│   │   │   ├── categories/        # 文章类型模板
│   │   │   ├── styles/            # 写作风格参考
│   │   │   ├── topics/            # 主题类别定义
│   │   │   ├── samples/           # 示例文章
│   │   │   ├── user-input/        # 用户输入素材
│   │   │   └── archives/          # 生成文章归档
│   │   ├── ddd-coding-standards/  # DDD 代码规范
│   │   │   ├── SKILL.md           # 技能定义
│   │   │   ├── README.md          # 详细文档
│   │   │   ├── specs/             # 规范详情
│   │   │   └── examples/          # 示例输出
│   │   └── sequence-diagram-generator/  # 时序图生成器
│   │       ├── SKILL.md           # 技能定义
│   │       ├── README.md          # 详细文档
│   │       ├── scripts/           # 生成脚本
│   │       └── samples/           # 示例图片
│   │
│   └── commands/                  # Commands 目录
│       ├── intro.md               # 项目分析与 README 生成
│       ├── new-command.md         # 创建新命令
│       ├── new-skill.md           # 创建新技能
│       ├── modify-skill.md        # 修改现有技能
│       ├── git-commit.md          # 智能 git commit
│       ├── git-push.md            # 推送到远程仓库
│       ├── new-blog.md            # 快速创建博客
│       └── new-sequence-diagram.md  # 快速创建时序图
│
├── AGENTS.md                      # 开发工作指南
└── README.md                      # 本文件
```

## 开发指南

### 文件规范

#### Skill 文件 (`.opencode/skills/{name}/SKILL.md`)

```yaml
---
name: skill-name
description: 一句话描述技能用途
argument-hint: "[可选参数]"
disable-model-invocation: true      # 可选：仅用户可调用
allowed-tools: Read, Glob, Grep     # 可选：限制可用工具
---
```

#### Command 文件 (`.opencode/commands/{name}.md`)

```yaml
---
agent: build
description: 命令的一句话描述
---
```

### 设计原则

1. **简洁性** - 文件建议不超过 100 行
2. **描述清晰** - description 包含关键词，帮助识别场景
3. **中文优先** - 面向中文用户场景
4. **交互式设计** - 适当使用 `question` 工具
5. **行动导向** - 使用祈使语气，结构化内容

## 贡献指南

欢迎贡献新的 Skills 或 Commands！

### 提交规范

```
<类型>: <描述>
```

**类型：** `feat`（新功能）、`fix`（修复）、`docs`（文档）、`refactor`（重构）

### 预提交检查

- [ ] SKILL.md 有有效的 YAML frontmatter
- [ ] description 字段准确描述用途
- [ ] 文件路径使用相对路径
- [ ] 新增 Command 已添加到 README 表格
- [ ] 新增 Skill 已添加到 README 表格

## 参考资源

- [Open Code Commands 文档](https://opencode.ai/docs/commands/)
- [Open Code Skills 文档](https://opencode.ai/docs/skills/)
- [Agent Skills 文档](https://agentskills.io/what-are-skills)
- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)

## 许可证

MIT License

---

**Happy Coding with AI!** 🤖✨
