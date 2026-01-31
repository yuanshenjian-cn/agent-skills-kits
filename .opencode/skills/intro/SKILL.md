---
name: intro
description: 智能项目分析助手。全面深度分析当前工程目录结构、技术栈、关键文件，自动生成专业的 README.md 文件来介绍项目。
argument-hint: "[可选：自定义README保存路径，默认当前目录]"
allowed-tools: Glob, Read, Grep, Write
---

# 项目介绍生成器

自动分析项目结构，理解技术栈和架构，生成专业的 README.md 介绍文档。

## 核心流程

### 第一步：扫描项目结构

使用 `Glob` 全面扫描项目：

```bash
# 根目录文件
glob: *.*

# 配置文件
glob: **/*.json
glob: **/*.yaml
glob: **/*.yml
glob: **/*.toml
glob: **/*.gradle
glob: **/pom.xml
glob: **/Makefile
glob: **/CMakeLists.txt
glob: **/setup.py
glob: **/pyproject.toml
glob: **/Cargo.toml
glob: **/go.mod
glob: **/package.json
glob: **/requirements.txt
glob: **/Gemfile

# 源代码目录
glob: **/src/**
glob: **/lib/**
glob: **/app/**
glob: **/main/**
glob: **/test/**
glob: **/tests/**
glob: **/docs/**
glob: **/examples/**

# 特殊文件
glob: **/Dockerfile*
glob: **/docker-compose*
glob: **/.env*
glob: **/LICENSE*
glob: **/CONTRIBUTING*
glob: **/.gitignore
glob: **/AGENTS.md
glob: **/.opencode/**
```

### 第二步：识别项目类型

根据文件特征判断项目类型：

**前端项目**：
- package.json → 检查 dependencies（React/Vue/Angular/Next.js/Nuxt.js/Svelte）
- vite.config.* / webpack.config.* / rollup.config.*
- tsconfig.json
- 框架特定文件（如 next.config.js, nuxt.config.ts）

**后端项目**：
- Java: pom.xml / build.gradle, Spring Boot application.properties/yaml
- Node.js: package.json（Express/NestJS/Fastify/Koa）
- Python: requirements.txt / setup.py / pyproject.toml（Flask/Django/FastAPI）
- Go: go.mod, main.go
- Rust: Cargo.toml
- Ruby: Gemfile

**全栈项目**：
- 同时包含前后端特征
- monorepo 结构（lerna.json, nx.json, pnpm-workspace.yaml）

**CLI/工具项目**：
- bin/ 目录
- CLI 框架依赖（commander.js, cobra, click, clap）
- 全局安装配置

**库/框架项目**：
- 缺少应用入口点
- 导出入口定义
- 示例代码目录

**移动端项目**：
- iOS: .xcodeproj, Podfile
- Android: build.gradle, AndroidManifest.xml
- 跨平台: pubspec.yaml (Flutter), capacitor.config (Capacitor)

**DevOps/基础设施**：
- Dockerfile, docker-compose.yml
- .github/workflows/
- Terraform: *.tf
- Ansible: *.yml, inventory
- Kubernetes: *.yaml (deployments, services)

### 第三步：读取关键文件

根据项目类型，读取核心文件理解项目：

**必须读取的文件**（优先级最高）：

| 文件类型 | 读取内容 |
|---------|---------|
| package.json | name, description, version, scripts, dependencies, devDependencies |
| pyproject.toml/setup.py | 项目元数据、依赖、入口点 |
| Cargo.toml | name, version, dependencies, features |
| go.mod | module 路径、Go 版本、依赖 |
| pom.xml/build.gradle | artifactId, groupId, version, dependencies, plugins |
| Gemfile | gems, ruby 版本 |
| tsconfig.json | TypeScript 配置、编译目标 |
| README.md（如果存在） | 现有内容，避免重复，寻找补充信息 |
| LICENSE | 许可证类型 |

**重要配置文件**：
- Docker 相关：理解部署方式
- CI/CD 配置：GitHub Actions, GitLab CI, Jenkins
- 环境配置：.env.example, config/
- 测试配置：jest.config, pytest.ini, test files

**源代码结构**：
- 主要入口文件：src/index.*, main.*, app.*
- 核心模块目录结构
- 测试目录结构

### 第四步：深度分析

#### 4.1 技术栈识别

**识别使用的技术**：
- 编程语言（根据文件扩展名统计）
- 主要框架和库
- 数据库（从依赖或配置识别：Prisma, SQLAlchemy, MongoDB 驱动等）
- 部署平台（Vercel, AWS, Docker, Kubernetes 等）

**识别架构模式**：
- MVC / MVVM / 分层架构
- 微服务 / 单体应用
- Serverless / 传统部署
- 事件驱动 / REST API / GraphQL

#### 4.2 项目功能理解

**从代码推断功能**：
- 读取主要源代码文件的注释和结构
- 分析路由/API 端点定义
- 查看数据库模型/实体定义
- 理解主要业务流程

**从配置推断功能**：
- scripts 字段中的命令
- 环境变量定义
- 文档中的使用说明

#### 4.3 项目特点总结

提取项目独特之处：
- 解决的问题
- 核心功能
- 技术亮点
- 架构优势
- 使用场景

### 第五步：生成 README.md

#### 5.1 内容结构

专业的 README 应包含以下部分：

```markdown
# [项目名称]

> [一句话描述项目的核心价值和用途]

## 简介

[详细描述项目背景、解决的问题、主要功能]

## 技术栈

- **核心语言**: [语言]
- **主要框架**: [框架]
- **数据库**: [数据库]
- **部署**: [部署方式]
- [其他重要技术]

## 功能特性

- [核心功能 1]
- [核心功能 2]
- [核心功能 3]
- [更多...]

## 项目结构

```
[目录树，展示主要结构]
```

## 快速开始

### 环境要求

- [Node.js >= 18](link)
- [Python >= 3.9](link)
- [其他依赖]

### 安装

```bash
# 克隆项目
git clone [repository-url]
cd [project-name]

# 安装依赖
[具体安装命令]

# 环境配置
cp .env.example .env
# 编辑 .env 配置必要的环境变量
```

### 运行

```bash
# 开发模式
[开发命令]

# 生产构建
[构建命令]

# 运行测试
[测试命令]
```

## 配置说明

[重要的配置项说明]

## API 文档 / 使用指南

[如果是 API 项目，提供主要端点说明]
[如果是工具项目，提供使用示例]

## 开发指南

### 开发规范

- [代码规范]
- [提交规范]
- [分支策略]

### 目录说明

- `src/` - [说明]
- `tests/` - [说明]
- [其他重要目录]

## 部署

[部署方式和步骤]

## 贡献指南

参考 [CONTRIBUTING.md](./CONTRIBUTING.md)

## 许可证

[LICENSE](./LICENSE)
```

#### 5.2 内容定制规则

根据项目类型调整内容：

**前端应用**：
- 添加预览/截图部分
- 详细的浏览器兼容性说明
- 性能优化特性
- UI 框架说明

**后端 API**：
- 详细的 API 端点文档
- 认证/授权说明
- 数据库 Schema 说明
- Postman/Swagger 链接

**CLI 工具**：
- 安装命令（全局/本地）
- 所有命令和参数的完整列表
- 使用示例和场景
- 配置文件的说明

**库/框架**：
- 设计理念和架构
- 详细的 API 文档
- 示例代码
- 与其他库的对比

**DevOps 工具**：
- 架构图
- 部署流程图
- 配置参数详解
- 故障排除指南

### 第六步：保存文件

使用 `Write` 工具保存 README.md：

**保存路径**：
- 默认：当前工作目录下的 `README.md`
- 用户指定：使用用户提供的参数作为路径

**文件名处理**：
- 如果已存在 README.md，询问用户是否覆盖或备份为 README.old.md
- 确保文件名正确（区分大小写）

## 质量标准

生成的 README 必须满足：

- **准确性**：所有技术信息必须正确
- **完整性**：涵盖项目的主要方面
- **清晰度**：结构清晰，易于理解
- **实用性**：包含实际可用的命令和步骤
- **专业性**：使用标准的开源项目文档格式
- **一致性**：与项目实际结构和配置一致

## 特殊情况处理

### 空项目/新项目

如果检测到项目几乎没有文件：
- 生成基础模板 README
- 提示用户需要完善的内容
- 提供待办清单

### 已有 README.md

如果 README.md 已存在：
- 读取现有内容
- 分析缺失的部分
- 询问用户：覆盖 / 补充 / 备份后重写

### 复杂项目（monorepo）

如果是 monorepo 结构：
- 生成根目录概述 README
- 建议每个子包有自己的 README
- 提供项目间关系的说明

### 私有/内部项目

如果检测到私有仓库特征：
- 移除贡献指南部分
- 添加内部联系信息
- 强调访问权限说明

## 开始分析

现在按照流程分析项目并生成 README.md。
