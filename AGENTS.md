# Agent Skills Kits - AGENTS.md

这个仓库包含用于各种任务的 Claude Code 技能。本文档帮助代理在这个代码库中有效工作。

## 项目概述

这是一个 Claude Code 技能集合，用于扩展代理的能力：

1. **blog-writer** - 智能博客写作助手，支持交互式模式选择
2. **create-or-update-skill** - 创建 Claude Code 技能文件的指南
3. **ddd-coding-standards** - Java + SpringBoot + Gradle 项目的 DDD 分层架构代码规范

## 构建/检查/测试命令

### 技能基于 Markdown，无需构建

这个项目**没有需要构建或测试的代码**。技能使用 Markdown 和 YAML frontmatter 定义在 `SKILL.md` 文件中。

### 验证命令

验证技能文件：

```bash
# 检查所有 SKILL.md 文件是否存在且可读
find .opencode/skills -name "SKILL.md" -type f

# 验证 YAML frontmatter 语法
grep -A 5 "^---$" .opencode/skills/*/SKILL.md
```

### 测试技能

技能通过在 **Claude Code 中使用**来测试：

```bash
# 安装技能到你的 Claude Code 技能目录
cp -r .opencode/skills/* ~/.opencode/skills/

# 测试技能
/blog-writer "我的主题" --type=技术分享 --style=简洁明了
/ddd-coding-standards "审查我的代码"
```

## 代码风格指南

### 通用原则

1. **Markdown 优先**：所有技能都使用 Markdown 和 YAML frontmatter 编写
2. **中文语言**：技能使用中文编写，面向中文用户
3. **交互式设计**：技能在适当时使用 `question` 工具进行用户交互
4. **清晰结构**：遵循一致的章节组织

### YAML Frontmatter 格式

```yaml
---
name: skill-name
description: 技能用途的一句话描述
argument-hint: "[可选] [参数]"
allowed-tools: Read, Glob, Grep, question, Write
disable-model-invocation: true  # 可选：仅用户可调用
---
```

**命名约定：**
- `name`：kebab-case，仅小写字母、数字、连字符
  - ✅ `blog-writer`, `ddd-coding-standards`
  - ❌ `BlogWriter`, `ddd_coding_standards`
- `description`：清晰、简洁，包含关键词用于自动加载
- `allowed-tools`：逗号分隔的允许工具列表

### Markdown 内容指南

#### 章节结构

1. **标题**：`# Skill Name` (H1)
2. **核心指令**：清晰、可操作的指导
3. **示例**：展示使用模式
4. **边缘情况**：处理特殊场景

#### 写作风格

- **简洁**：避免不必要的前言
- **行动导向**：使用祈使语气（"读取文件"、"检查结构"）
- **结构化**：使用项目符号、编号列表、代码块
- **具体**：引用确切的文件路径、函数名、行号

### 工具使用指南

#### 各技能的允许工具

**blog-writer:**
- `Read` - 读取示例文章、模板、用户输入
- `Glob` - 在 categories、styles、topics 中查找文件
- `Grep` - 在示例中搜索模式
- `question` - 交互式模式选择
- `Write` - 保存文章到 archives

**ddd-coding-standards:**
- `Read` - 读取代码文件进行分析
- `Glob` - 在项目中查找 Java 文件
- `Grep` - 在代码中搜索模式
- `SearchCodebase` - 搜索代码库上下文

**create-or-update-skill:**
- 无工具允许（使用 `disable-model-invocation: true`）

#### 工具调用最佳实践

1. **批量操作**：在独立操作时并行使用多个工具调用
2. **错误处理**：读取前检查文件是否存在
3. **路径处理**：使用绝对路径或相对于项目根目录的路径
4. **输出限制**：注意大输出的截断

### 目录结构

```
agent-skills-kits/
├── .opencode/
│   └── skills/
│       ├── blog-writer/
│       │   ├── SKILL.md          # 主技能文件
│       │   ├── README.md         # 详细文档
│       │   ├── archives/         # 已保存的文章
│       │   ├── categories/       # 文章类型模板
│       │   ├── samples/          # 示例文章
│       │   ├── styles/           # 风格指南
│       │   ├── topics/           # 主题分类
│       │   └── user-input/       # 用户素材目录
│       ├── create-or-update-skill/
│       │   └── SKILL.md
│       └── ddd-coding-standards/
│           ├── SKILL.md
│           ├── examples/         # 输出示例
│           └── specs/            # 规范文件
├── README.md
└── AGENTS.md                    # 本文档
```

### Git 工作流

#### 提交信息格式

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
- `test`：添加测试
- `chore`：维护任务

**示例：**
```
feat: 添加支持交互式模式的 blog-writer 技能

- 支持 4 个主题类别
- 支持 7 种文章类型
- 支持 5 种写作风格
- 保存文章到 archives 目录
```

```
fix: 修正 ddd-coding-standards 层依赖规则

- 修复接口层依赖方向
- 更新检查命令
```

#### 预提交检查清单

1. 所有 SKILL.md 文件都有有效的 YAML frontmatter
2. 文档中没有损坏的内部链接
3. 代码示例语法正确
4. 文件路径准确
5. 中文文本格式正确

### 测试清单

测试技能时：

- [ ] 技能加载无错误
- [ ] 交互式提示工作正常
- [ ] 工具权限正确
- [ ] 文件路径解析正常
- [ ] 输出格式符合规范
- [ ] 边缘情况已处理
- [ ] 错误信息清晰

### 安全指南

1. **无机密**：绝不包含 API 密钥、密码、令牌
2. **文件路径**：使用相对路径，避免绝对路径
3. **用户输入**：验证和清理所有用户输入
4. **外部数据**：验证来自 `user-input/` 目录的数据
5. **工具权限**：限制为最小必需工具

### 参考资源

- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)
- [Agent Skills 标准](https://agentskills.io)
- [README.md](./README.md) - 项目概述
