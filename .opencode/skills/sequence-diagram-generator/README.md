# 时序图生成器

将 Mermaid 时序图代码转换为 PNG 图片的工具。

## 安装

```bash
# 复制技能到 opencode 技能目录
cp -r .opencode/skills/sequence-diagram-generator ~/.opencode/skills/
```

## 使用

在 Claude Code 中使用：

```
/sequence-diagram-generator
```

然后粘贴你的时序图代码，例如：

```
sequenceDiagram
    actor U as 用户
    participant S as 系统
    U->>S: 登录请求
    S->>S: 验证身份
    S-->>U: 返回 token
```

## 技术说明

本工具使用 [Kroki.io](https://kroki.io/) 在线服务生成图片：
- 免费、无需注册
- 支持多种图表类型
- 无需本地安装依赖

## 本地开发

如果你想在本地运行生成脚本：

```bash
cd .opencode/skills/sequence-diagram-generator
node generate.js diagram.mmd
```

## 示例输出

输入代码后，工具会生成类似 `sequence_20250131_143022.png` 的图片文件。
