---
name: create-or-update-skill
description: 创建或更新符合 Claude Code/OpenCode skill 规范的 SKILL.md 文件
disable-model-invocation: true
---

# Skill 创建指南

创建符合 Claude Code/OpenCode 规范的 skill 文件。

## Skill 结构

### 目录结构

```
my-skill/
├── SKILL.md              # 必需：技能主要指令
├── README.md             # 可选：详细文档
├── reference.md          # 可选：参考材料
├── template.md           # 可选：Claude 填充的模板
├── examples/             # 可选：示例输出
│   └── sample.md
└── scripts/              # 可选：辅助脚本
    └── helper.py
```

### SKILL.md 格式

```yaml
---
name: my-skill-name
description: 一句话描述技能用途
---

# 技能名称

## 指令
[核心指导内容]
```

## Frontmatter 字段

### Claude Code 规范

| 字段 | 说明 |
|------|------|
| `name` | 技能名称（kebab-case，最多64字符）。省略时使用目录名 |
| `description` | **必需**：技能功能描述，Claude 用它决定何时加载 |
| `disable-model-invocation` | `true` = 仅用户可手动调用 |
| `user-invocable` | `false` = 仅 Claude 可调用，不在 `/` 菜单显示 |
| `allowed-tools` | 限制可用工具，如 `Read, Grep, Glob, question` |
| `context` | `fork` = 在子代理中运行（有独立上下文） |

### OpenCode 扩展规范

| 字段 | 说明 |
|------|------|
| `agent` | 指定子代理类型：`Explore`, `General` 等 |
| `argument-hint` | 参数提示，如 `[feature-name]` |
| `tokens-num` | 代码搜索时的默认 token 数 |

## 内容类型

**参考型技能**（Claude 自动加载）：
```yaml
---
description: API 设计规范和命名约定
---

编写 API 时：
- 使用 RESTful 命名
- 返回一致的错误格式
```

**任务型技能**（用户手动调用）：
```yaml
---
description: 部署应用到生产环境
disable-model-invocation: true
---

部署步骤：
1. 运行测试
2. 构建应用
3. 推送到生产环境
4. 验证部署
```

**带参数的技能**：
```yaml
---
description: 解释文件的功能和结构
---

解释文件 $ARGUMENTS：
1. 读取文件内容
2. 分析结构和目的
3. 总结功能
```

使用：`/explain-file src/main.py`

## 最佳实践

1. **保持简洁**：SKILL.md 建议不超过 100 行
2. **描述清晰**：description 包含关键词，帮助识别使用场景
3. **合理拆分**：详细材料放到单独文件（如 reference.md）
4. **正确控制**：需手动控制的任务设置 `disable-model-invocation: true`
5. **引用辅助文件**：在 SKILL.md 中用 `[reference.md](reference.md)` 引用

## 参考

- [OpenCode Skills 规范](https://opencode.ai/docs/skills/)
- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)
- [Agent Skills 标准](https://agentskills.io)
