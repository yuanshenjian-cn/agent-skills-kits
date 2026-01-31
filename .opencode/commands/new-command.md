---
description: 根据用户描述创建新的 agent command，自动提炼命令名称和描述，与用户确认后生成命令文件
argument-hint: "[功能描述]"
---

基于用户提供的功能描述，自动提炼 command 名称和功能说明，经过用户确认后创建 agent command 文件。

## 执行流程

### 1. 获取功能描述

**检查用户是否提供描述参数**（通过 $ARGUMENTS）：
- 已提供 → 使用提供的描述作为功能说明
- 未提供 → 使用 `question` 工具提示用户输入功能描述

### 2. 提炼命令信息

**根据功能描述智能提炼**：
- **命令名称**（kebab-case）：
  - 从描述中提取核心功能关键词
  - 转换为小写，用连字符连接
  - 示例：`生成项目 README` → `generate-readme`
  - 示例：`代码质量检查` → `code-quality-check`
  
- **Frontmatter 描述**：
  - 一句话概括命令的核心用途
  - 包含关键词便于识别使用场景
  - 简洁明了，不超过 50 字

- **Agent 类型**（可选）：
  - 判断是否需要指定子代理类型（如 `build`, `explore`, `general`）
  - 复杂任务可设置 `agent: build`

**向用户确认**：
```
根据您的描述，我提炼了以下命令信息：

命令名称：{command-name}
描述：{description}
Agent 类型：{agent-type}

是否确认创建？
[确认并创建] [修改名称] [修改描述] [取消]
```

**处理用户反馈**：
- 确认并创建 → 进入步骤 3
- 修改名称 → 询问用户期望的名称 → 重新确认
- 修改描述 → 询问用户更准确的描述 → 重新提炼 → 重新确认
- 取消 → 结束流程

### 3. 创建命令文件

**生成命令文件**：

```yaml
---
description: {提炼的描述}
---

{根据描述生成的核心指令和流程说明}

## 执行流程

### 1. 步骤一

[步骤说明]

### 2. 步骤二

[步骤说明]

## 使用示例

```
/{command-name} [参数]
```
```

**目标路径**：`.opencode/commands/{command-name}.md`

### 4. 完成通知

**创建成功后**：
```
✅ Agent Command "{command-name}" 创建成功！

文件位置：.opencode/commands/{command-name}.md

使用方式：
/{command-name}  # 调用命令
```

## Agent Command 规范

### 文件位置
- 所有 agent command 文件位于 `.opencode/commands/` 目录
- 文件名使用 kebab-case（如 `new-skill.md`, `code-review.md`）

### Frontmatter 字段

```yaml
---
description: 命令的一句话描述，包含关键词便于识别
agent: build              # 可选：指定子代理类型
argument-hint: "[参数]"   # 可选：参数提示
tokens-num: 5000          # 可选：代码搜索 token 数
allowed-tools: Read       # 可选：限制可用工具
---
```

### Agent 类型说明
- `build`：构建/生成类任务（代码生成、文档生成等）
- `explore`：探索/分析类任务（代码库探索、项目分析等）
- `general`：通用任务（默认）

### 质量标准
- 命令名称使用 kebab-case（小写字母 + 连字符）
- 描述包含关键词，便于识别使用场景
- 内容结构清晰，包含执行流程章节
- 提供使用示例

现在开始创建新 agent command。
