# Agent Skills Kits

> 一组精心设计的 Claude Code / Opencode 技能，扩展 AI 助手在博客写作、代码规范、技能创建等领域的专业能力。

## 简介

Agent Skills Kits 是一个开源的 AI 技能集合，旨在为 Claude Code 和 Opencode 用户提供即插即用的专业能力扩展。每个技能都是一个独立的、自包含的模块，通过 YAML frontmatter 和 Markdown 定义，可在 AI 助手中自动加载或手动调用。

**核心价值：**
- 🚀 **即装即用** - 复制技能目录到 `.opencode/skills/` 即可使用
- 📝 **中文优先** - 所有技能针对中文用户场景设计
- 🎯 **专业聚焦** - 每个技能专注于特定领域的专业能力
- 🛠️ **可扩展** - 基于标准规范，易于创建和定制新技能

## 可用命令

| 命令 | 描述 | 参数 | 类型 |
|------|------|------|------|
| `/blog-writer` | 智能博客写作助手 | `[主题] --type=[类型] --style=[风格] --words=[字数]` | Agent Command |
| `/ddd-coding-standards` | DDD 分层架构代码规范检查 | `[代码/问题描述]` | Agent Command |
| `/sequence-diagram-generator` | 将 Mermaid 时序图转换为 PNG 图片 | `[时序图代码]` | User Command |
| `/create-or-update-skill` | 创建或更新 Skill 文件的完整指南 | `[技能名称]` | Agent Command |

## 技能清单

### 1. Blog Writer - 智能博客写作助手

专业博客文章写作助手，支持交互式配置，智能学习示例风格，输出高质量文章。

**核心功能：**
- 🎨 **5 大主题类别**：运动健康、投资理财、生活杂谈、软件开发、脱口秀
- 📄 **7 种文章类型**：技术分享、产品分析、观点评论、学习笔记、工具推荐、经验分享、成长故事
- ✍️ **6 种写作风格**：故事叙述、朴实稳重、轻松幽默、简洁明了、专业严谨、浮夸搞笑
- 📊 **灵活字数控制**：500 字到 10000 字，适配不同阅读场景
- 🧠 **智能风格学习**：自动分析示例文章，学习写作风格
- 💾 **自动归档保存**：生成文章自动保存到 archives 目录

**目录结构：**
```
blog-writer/
├── SKILL.md              # 技能核心定义
├── README.md             # 详细使用说明
├── archives/             # 已保存的文章
│   └── *.md             # 生成的文章文件
├── categories/           # 文章类型模板（7种）
│   ├── 技术分享.md
│   ├── 产品分析.md
│   ├── 观点评论.md
│   ├── 学习笔记.md
│   ├── 工具推荐.md
│   ├── 经验分享.md
│   └── 成长故事.md
├── samples/              # 示例文章（按主题分类）
├── styles/               # 写作风格指南（6种）
│   ├── 故事叙述.md
│   ├── 朴实稳重.md
│   ├── 轻松幽默.md
│   ├── 简洁明了.md
│   ├── 专业严谨.md
│   └── 浮夸搞笑.md
├── topics/               # 主题分类定义（5种）
│   ├── 运动健康.md
│   ├── 投资理财.md
│   ├── 生活杂谈.md
│   ├── 软件开发.md
│   └── 脱口秀.md
└── user-input/           # 用户素材目录
    └── *.md             # 用户提供的写作素材
```

**使用示例：**
```bash
/blog-writer "如何开始学习投资" --type=经验分享 --style=朴实稳重 --words=2000
```

---

### 2. DDD Coding Standards - DDD 分层架构代码规范

Java + SpringBoot + Gradle 项目的 DDD（领域驱动设计）分层架构代码规范专家，在编码时提供实时规范指导。

**核心规范：**
- 🏗️ **标准 DDD 四层架构**：接口层、应用层、领域层、基础设施层
- 📐 **依赖方向控制**：严格的层间依赖规则，防止架构腐化
- 🎯 **领域层独立性**：核心领域逻辑完全独立，不依赖任何框架
- 📝 **命名规范**：杜绝模糊命名（data/info/util/manager/handler），代码自解释
- 🧪 **测试规范**：JUnit 5 + 真实对象注入（禁止内部组件 Mock）

**包含内容：**
- 分层架构定义和依赖规则
- 各层职责和约束清单
- 领域层独立性检查清单
- 命名规范和面向对象规范
- 自动化测试规范
- 常用检查命令（Grep 命令集合）

**目录结构：**
```
ddd-coding-standards/
├── SKILL.md              # 技能定义
├── README.md             # 详细说明
├── examples/             # 输出示例
│   └── sample-output.md
└── specs/                # 规范文件
    └── springboot.md
```

**使用示例：**
```bash
/ddd-coding-standards "审查这个 Controller 是否符合规范"
```

---

### 3. Sequence Diagram Generator - 时序图生成器

将 Mermaid 时序图代码转换为 PNG 图片，无需安装本地依赖。

**核心特性：**
- 🌐 **在线渲染**：使用 Kroki.io 免费服务，无需本地安装
- 🖼️ **PNG 格式输出**：高质量图片生成
- ⚡ **快速生成**：即时渲染，秒级输出
- 📁 **自定义目录**：支持指定保存位置
- 🔄 **备用方案**：支持 Mermaid Live Editor 作为备选

**目录结构：**
```
sequence-diagram-generator/
├── SKILL.md              # 技能定义
├── README.md             # 详细文档
├── scripts/              # 辅助脚本
│   ├── generate.js       # Node.js 生成脚本
│   └── generate.sh       # Bash 备用脚本
└── samples/              # 示例时序图
    └── *.png
```

**技术方案：**
- Kroki.io API 调用
- Node.js 生成脚本（`scripts/generate.js`）
- Bash 备用脚本（`scripts/generate.sh`）

**使用示例：**
```bash
/sequence-diagram-generator
# 然后粘贴 Mermaid 时序图代码
```

---

### 4. Create or Update Skill - Skill 创建指南

创建符合 Claude Code / Opencode 规范的 SKILL.md 文件的完整指南。

**核心内容：**
- 📋 **标准结构**：目录组织、文件命名、内容格式
- 📝 **Frontmatter 规范**：Claude Code 和 OpenCode 扩展字段详解
- 🎯 **技能类型**：参考型、任务型、带参数型技能的设计模式
- ✅ **最佳实践**：简洁性、描述清晰、合理拆分、脚本位置等

**适用场景：**
- 创建新的 AI 技能
- 更新现有技能文件
- 理解技能系统工作原理

**目录结构：**
```
create-or-update-skill/
└── SKILL.md              # 技能定义（包含完整规范指南）
```

**使用示例：**
```bash
/create-or-update-skill
```

---

## 项目结构

```
agent-skills-kits/
├── .opencode/
│   └── skills/
│       ├── blog-writer/                   # 博客写作助手
│       │   ├── SKILL.md                   # 技能定义
│       │   ├── README.md                  # 详细文档
│       │   ├── archives/                  # 已保存文章
│       │   ├── categories/                # 文章类型模板（7种）
│       │   ├── samples/                   # 示例文章
│       │   ├── styles/                    # 写作风格指南（6种）
│       │   ├── topics/                    # 主题分类（5种）
│       │   └── user-input/                # 用户素材
│       │
│       ├── create-or-update-skill/        # Skill 创建指南
│       │   └── SKILL.md                   # 技能定义
│       │
│       ├── ddd-coding-standards/          # DDD 代码规范
│       │   ├── SKILL.md                   # 技能定义
│       │   ├── README.md                  # 详细说明
│       │   ├── examples/                  # 输出示例
│       │   └── specs/                     # 规范文件
│       │
│       └── sequence-diagram-generator/    # 时序图生成器
│           ├── SKILL.md                   # 技能定义
│           ├── README.md                  # 详细文档
│           ├── scripts/                   # 辅助脚本
│           │   ├── generate.js            # Node.js 脚本
│           │   └── generate.sh            # Bash 脚本
│           └── samples/                   # 示例时序图
│
├── AGENTS.md                              # 代理工作指南（开发文档）
└── README.md                              # 项目说明（本文件）
```

---

## 使用方法

### 安装技能

将技能复制到你的 Claude Code / Opencode 技能目录：

```bash
# Claude Code
mkdir -p ~/.claude/skills
cp -r .opencode/skills/* ~/.claude/skills/

# Opencode
mkdir -p ~/.opencode/skills
cp -r .opencode/skills/* ~/.opencode/skills/
```

### 使用技能

在 Claude Code / Opencode 中使用 `/` 命令调用技能：

```bash
# 博客写作
/blog-writer "文章主题" --type=技术分享 --style=简洁明了

# DDD 代码规范检查
/ddd-coding-standards "审查代码"

# 生成时序图
/sequence-diagram-generator
# 然后粘贴 Mermaid 代码

# 创建新技能
/create-or-update-skill
```

### 开发新技能

参考 `create-or-update-skill` 技能中的规范，创建新的技能目录：

```bash
mkdir -p .opencode/skills/my-skill/scripts
# 编写 SKILL.md
# 添加辅助脚本（可选）
# 添加示例和文档（可选）
```

---

## 技能规范

所有技能遵循以下标准：

### 文件格式

- **SKILL.md** - 必需，包含 YAML frontmatter + Markdown 内容
- **README.md** - 可选，详细使用说明
- **scripts/** - 可选，辅助脚本目录
- **examples/** 或 **samples/** - 可选，示例文件目录

### YAML Frontmatter

```yaml
---
name: skill-name                          # 技能名称（kebab-case）
description: 一句话描述技能用途          # 必需：帮助 AI 识别使用场景
argument-hint: "[可选参数]"              # 参数提示
disable-model-invocation: true           # 可选：仅用户可手动调用
allowed-tools: Read, Glob, Grep          # 可选：限制可用工具
---
```

### 设计原则

1. **简洁性** - SKILL.md 建议不超过 100 行
2. **描述清晰** - description 包含关键词，帮助识别使用场景
3. **中文优先** - 面向中文用户场景设计
4. **交互式设计** - 适当使用 `question` 工具进行用户交互
5. **行动导向** - 使用祈使语气，结构化内容

---

## 贡献指南

欢迎贡献新的技能或改进现有技能！

### 提交规范

```
<类型>: <描述>

<可选正文>

<可选页脚>
```

**类型：**
- `feat`：新技能或功能
- `fix`：技能逻辑的 bug 修复
- `docs`：文档更新
- `style`：格式、命名等
- `refactor`：代码重构

### 预提交检查清单

- [ ] SKILL.md 有有效的 YAML frontmatter
- [ ] description 字段准确描述技能用途
- [ ] 文档中没有损坏的内部链接
- [ ] 文件路径准确（使用相对路径）
- [ ] 中文文本格式正确

### 测试技能

在提交前，请测试技能是否正常工作：

```bash
# 安装技能
mkdir -p ~/.opencode/skills
cp -r .opencode/skills/my-skill ~/.opencode/skills/

# 在 Claude Code / Opencode 中测试
/my-skill
```

---

## 参考资源

- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)
- [OpenCode Skills 规范](https://opencode.ai/docs/skills/)
- [Agent Skills 标准](https://agentskills.io)

---

## 许可证

MIT License - 详见 LICENSE 文件

---

**Happy Coding with AI!** 🤖✨
