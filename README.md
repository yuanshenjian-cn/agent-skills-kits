# Agent Skills Kits

Claude Code 技能集合，包含多个实用的辅助技能，提升开发效率。

## 包含技能

### 1. blog-writer
博客写作助手，帮助用户撰写高质量的博客文章。

**功能特点：**
- 支持多种文章类型（技术分享、产品分析、观点评论、学习笔记、工具推荐、面试经验）
- 多种写作风格（专业严谨、轻松幽默、简洁明了、故事叙述）
- 支持从头创作或基于已有内容扩展
- 灵活的字数控制（500/1000/2000/5000字）

**使用示例：**
```bash
/blog-writer SpringBoot实战教程 --type=技术分享 --style=简洁明了
/blog-writer --input=./my-ideas.md --type=技术分享 --style=专业严谨
```

### 2. create-or-update-skill
Skill 创建和更新指南，帮助创建符合 Claude Code 规范的技能文件。

**功能特点：**
- 完整的 Skill 结构说明
- Front Matter 字段详细解释
- 任务型和参考型技能示例
- 工具权限和调用控制指南

**使用场景：**
- 创建新的自定义技能
- 更新现有技能配置
- 了解 Claude Code 技能规范

### 3. ddd-layer-reviewer
SpringBoot DDD 分层架构代码规范检查工具。

**功能特点：**
- 检查分层结构是否符合 DDD 标准
- 验证依赖方向正确性
- 确保领域层完全独立
- 检查命名规范和包引用违规
- 生成详细的检查报告

**检查内容：**
- 分层结构分析（interface/application/domain/infrastructure）
- 依赖方向验证
- 领域层独立性检查（不依赖 Spring Web、持久化等框架）
- 类命名规范检查
- 包引用违规检测

**使用示例：**
```bash
/ddd-layer-reviewer /path/to/project
```

## 安装

将这些技能复制到你的 Claude Code 技能目录：

```bash
cp -r .opencode/skills/* ~/.opencode/skills/
```

## 项目结构

```
agent-skills-kits/
├── .opencode/
│   └── skills/
│       ├── blog-writer/
│       │   ├── examples/          # 写作风格示例
│       │   ├── templates/        # 文章类型模板
│       │   └── SKILL.md
│       ├── create-or-update-skill/
│       │   └── SKILL.md
│       └── ddd-layer-reviewer/
│           ├── examples/         # 输出示例
│           └── SKILL.md
└── README.md
```

## 贡献

欢迎提交新的技能或改进现有技能！

## 许可

MIT License
