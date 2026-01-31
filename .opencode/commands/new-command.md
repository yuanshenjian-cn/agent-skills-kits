---
description: 根据用户描述创建新的 opencode command，自动提炼命令名称，在 .opencode/commands/ 目录下生成 md 文件
argument-hint: "[功能描述]"
---

根据用户提供的功能描述，自动提炼 command 名称，在 `.opencode/commands/` 目录下创建简单的 `.md` 文件。

## 执行流程

### 1. 获取功能描述

**检查用户是否提供描述参数**（通过 $ARGUMENTS）：
- 已提供 → 使用提供的描述作为功能说明
- 未提供 → 使用 `question` 工具提示用户输入功能描述

### 2. 提炼命令名称

**根据功能描述提炼命令名称**（kebab-case）：
- 从描述中提取核心功能关键词
- 转换为小写，用连字符连接
- 示例：`生成项目 README` → `generate-readme`
- 示例：`代码质量检查` → `code-quality-check`

**向用户确认**：
```
根据您的描述，我提炼了以下信息：

命令名称：{command-name}
功能描述：{用户提供的描述}

将在 .opencode/commands/{command-name}.md 创建命令文件。

是否确认创建？
[确认并创建] [修改名称] [取消]
```

**处理用户反馈**：
- 确认并创建 → 进入步骤 3
- 修改名称 → 询问用户期望的名称 → 重新确认
- 取消 → 结束流程

### 3. 创建命令文件

**生成命令文件内容**：

```markdown
---
description: {提炼的简要描述}
---

{用户提供的功能描述}
```

**目标路径**：`.opencode/commands/{command-name}.md`

**使用 Write 工具创建文件**。

### 4. 完成通知

**创建成功后**：
```
✅ Command "{command-name}" 创建成功！

文件位置：.opencode/commands/{command-name}.md

使用方式：
/{command-name}  # 调用命令
```

## Command 规范

### 文件位置
- 所有 command 文件位于 `.opencode/commands/` 目录
- 文件名使用 kebab-case（如 `new-command.md`, `code-review.md`）
- 文件扩展名为 `.md`

### 文件结构

```markdown
---
description: 命令的一句话描述
---

具体的功能说明或执行逻辑
```

### 质量标准
- 命令名称使用 kebab-case（小写字母 + 连字符）
- 描述简洁明了
- 文件位于正确的目录（.opencode/commands/）

现在开始创建新 command。
