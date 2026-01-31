---
description: 分析代码变更，自动提炼英文 commit 信息，提交前与用户确认
argument-hint: "[可选: 提交描述提示]"
---

分析当前工作区代码变更，智能生成符合 Conventional Commits 规范的英文提交信息，执行前与用户确认。

## 执行流程

### 1. 获取用户提示（可选）

**检查是否提供参数**（通过 `$ARGUMENTS`）：
- 已提供 → 作为提交信息生成的参考提示
- 未提供 → 完全基于代码变更自动分析

### 2. 检查工作区状态

使用 Bash 工具执行：
```bash
git status
git diff --stat
git diff HEAD
```

**判断状态**：
- 无变更 → 通知用户 "没有需要提交的更改"，结束流程
- 有变更 → 进入步骤 3

### 3. 分析变更内容

**统计变更**：
- 新增文件数
- 修改文件数
- 删除文件数
- 文件类型分布

**分析变更性质**：
- 新增功能 → `feat`
- Bug 修复 → `fix`
- 文档更新 → `docs`
- 代码重构 → `refactor`
- 配置/杂项 → `chore`
- 样式调整 → `style`
- 测试相关 → `test`

### 4. 生成提交信息

**结合用户提示（如有）和变更分析**：

**信息格式**：
```
<type>: <英文描述>
```

**生成规则**：
- 使用现在时态（add, fix, update 而非 added, fixed）
- 首字母小写
- 简洁明了，不超过 72 字符
- 英文描述

**示例**：
- 添加新功能 → `feat: add user login authentication`
- 修复问题 → `fix: resolve data validation error`
- 更新文档 → `docs: update readme with setup instructions`
- 重构代码 → `refactor: simplify error handling logic`

### 5. 用户确认

**展示变更统计和建议提交信息**：
```
📊 变更统计：
- 新增：{n} 个文件
- 修改：{n} 个文件
- 删除：{n} 个文件

📝 建议提交信息：{type}: {description}

是否执行 commit？
[确认提交] [修改信息] [取消]
```

**处理用户选择**：
- **确认提交** → 执行 `git commit -m "message"`
- **修改信息** → 使用 `question` 工具让用户输入新信息，然后重新确认
- **取消** → 结束流程，不执行提交

### 6. 执行提交并反馈

**执行命令**：
```bash
git commit -m "{message}"
```

**成功反馈**：
```
✅ Commit 成功！

提交信息：{message}
提交哈希：{commit-hash}
```

**失败处理**：
- 显示错误信息
- 提供解决建议

## 使用示例

```
/commit                          # 完全自动分析
/commit "添加用户认证模块"       # 提供提示辅助分析
/commit "fix memory leak"        # 直接提供英文描述
```

## 规范要求

- 提交信息使用英文
- 遵循 Conventional Commits 规范（feat/fix/docs/refactor/chore/style/test）
- 提交前必须经用户确认
- 展示详细的变更统计供参考
- 支持用户自定义提交信息