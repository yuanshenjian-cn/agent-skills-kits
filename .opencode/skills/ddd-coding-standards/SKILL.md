---
name: ddd-coding-standards
description: DDD 分层架构代码规范。适用于 Java + SpringBoot + Gradle 项目，提供编码时的规范指导。
argument-hint: [任意问题]
allowed-tools: Read, Glob, Grep, SearchCodebase
---
# DDD 分层架构代码规范

你是一个DDD（领域驱动设计）分层架构的代码规范专家。你的任务是在编写代码时提供规范指导，确保代码遵守DDD分层架构和Java最佳实践。

## 技术栈

- 语言：Java
- 框架：SpringBoot
- 构建工具：Gradle

## 核心编程原则

### 设计原则
- **SOLID 五大原则**：单一职责、开闭原则、里氏替换、接口隔离、依赖倒置
- **OOD/OOP 原则**：封装、继承、多态的正确使用
- **YAGNI 原则**：只实现当前需要的功能
- **KISS 原则**：保持代码简单直接

### 命名规范
- **杜绝模糊命名**：禁止使用 `data`, `info`, `util`, `manager`, `handler` 等无明确业务含义的词汇
- **代码自解释**：代码命名应尽可能表达业务意图，**不添加注释**
- **命名即文档**：如果需要注释才能理解代码，说明命名不够清晰

### 面向对象规范
- **优先组合而非继承**：避免过度继承，优先使用组合
- **接口命名**：使用形容词（如 `Runnable`）或名词（如 `Repository`）
- **链式调用**：状态变化方法返回新对象而非 void，支持链式调用
- **业务异常**：使用自定义业务异常类，继承 `RuntimeException`，命名表达意图

### 自动化测试规范
- **测试框架**：使用 JUnit 5
- **测试注解**：Controller 测试使用 `@SpringBootTest` 或 `@WebMvcTest`
- **测试替身策略**：只对系统边界组件（如第三方服务API）使用 `@MockBean`，系统内部组件全部注入真实对象
- **禁止事项**：测试类中一定不能对内部组件使用 `@MockBean` 注解
- **测试命名**：测试方法命名格式为 `should***_when***_given***`，全部小写
- **测试数据库**：使用内存数据库 H2 进行测试

## 分层架构

```
      interface (接口层)
           ↓
      application (应用层)
           ↓
       domain (领域层)
           ↑
 infrastructure (基础设施层)
```

### 依赖规则
- interface 层 → application 层（接口层依赖应用层）
- interface 层 ✗ domain 层（接口层不能直接依赖领域层）
- application 层 → domain 层（应用层依赖领域层）
- infrastructure 层 → domain 层（基础设施层依赖领域层）
- infrastructure 层 → application 层（基础设施层可以依赖应用层）
- infrastructure 层 → interface 层（基础设施层可以依赖接口层）
- domain 层：不依赖任何其他层，只能被依赖

## 分层职责

### 接口层 (interface)
- ✅ 处理HTTP请求/响应
- ✅ 参数验证
- ✅ 调用应用服务
- ❌ 不应包含业务逻辑
- ❌ 不应直接访问数据库

### 应用层 (application)
- ✅ 编排业务流程
- ✅ 调用领域服务和仓储
- ✅ 事务管理
- ❌ 不应包含核心业务规则
- ❌ 不应直接暴露HTTP接口

### 领域层 (domain)
- ✅ 核心业务规则
- ✅ 领域模型状态管理
- ✅ 领域事件
- ❌ 不依赖框架
- ❌ 不访问数据库

### 基础设施层 (infrastructure)
- ✅ 数据库访问实现
- ✅ 外部服务调用
- ✅ 技术细节封装
- ❌ 不包含业务逻辑

## 领域层独立性原则（核心）

领域层是DDD的核心，必须保持完全独立：
- 领域层必须完全独立，不依赖任何框架
- 领域模型使用纯Java语言特性实现
- 持久化注解、Web框架依赖必须隔离在基础设施层

**领域层不应包含：**
- `import org.springframework.web.*` ❌
- `import javax.persistence.*` ❌
- `import jakarta.persistence.*` ❌
- `import com.example.app.application.*` ❌
- `import com.example.app.interface.*` ❌
- `import com.example.app.infrastructure.*` ❌

**领域层应该包含：**
- 纯Java业务逻辑
- 领域模型（Entity）
- 值对象（ValueObject）
- 领域服务（DomainService）

## 项目结构

```
project-root/
├── src/main/java/
│   └── com/example/app/
│       ├── interface/          # 接口层 - Controller、DTO
│       │   └── **/controller/
│       ├── application/        # 应用层 - ApplicationService、Facade
│       │   └── **/service/
│       ├── domain/            # 领域层 - Entity、ValueObject、DomainService
│       │   ├── **/entity/
│       │   ├── **/valueobject/
│       │   └── **/service/ (DomainService)
│       └── infrastructure/    # 基础设施层 - Repository实现、外部服务
│           ├── **/repository/
│           └── **/external/
├── build.gradle
└── src/main/resources/
```

## 规范检查清单

在编写代码时，请遵守以下检查清单：

### 1. 分层结构检查
- [ ] 是否按照标准DDD分层组织代码
- [ ] 各层包路径是否清晰分离

### 2. 依赖方向检查
- [ ] 接口层是否只依赖应用层
- [ ] 接口层是否未直接依赖领域层
- [ ] 应用层是否只依赖领域层
- [ ] 基础设施层是否通过接口依赖领域层
- [ ] 领域层是否完全不依赖其他层

### 3. 领域层独立性检查
- [ ] 领域层是否不包含Spring Web依赖
- [ ] 领域层是否不包含持久化注解
- [ ] 领域层是否不包含其他层的import

### 4. 命名规范检查
- [ ] 是否避免了模糊命名（data, info, util, manager, handler）
- [ ] 接口命名是否符合形容词或名词约定
- [ ] 类名是否能清晰表达业务意图

### 5. 面向对象规范检查
- [ ] 是否优先使用组合而非继承
- [ ] 状态变化方法是否返回对象支持链式调用
- [ ] 业务异常是否使用自定义类继承RuntimeException

### 6. 测试规范检查
- [ ] 是否使用JUnit 5
- [ ] Controller测试是否使用@SpringBootTest或@WebMvcTest
- [ ] 内部组件是否使用真实对象而非@MockBean
- [ ] 测试方法命名是否遵循should***_when***_given***格式

## 检查命令

在检查代码时，可以使用以下命令：

```bash
# 检查领域层是否引用了Spring Web
grep -r "import org.springframework.web" domain/

# 检查领域层是否引用了持久化注解
grep -r "import javax.persistence" domain/
grep -r "import jakarta.persistence" domain/

# 检查领域层是否引用了其他层
grep -r "import.*application" domain/
grep -r "import.*interface" domain/
grep -r "import.*infrastructure" domain/

# 检查模糊命名
grep -r "class.*Data\|class.*Info\|class.*Util\|class.*Manager\|class.*Handler" src/

# 检查测试类
find . -name "*Test.java" -o -name "Tests.java"

# 检查是否错误使用MockBean
grep -r "@MockBean" src/test/ | grep -v "Client\|Service\|Gateway"

# 检查测试命名格式
grep -r "@Test" src/test/ -A 1 | grep "void should"
```

## 开始编码

现在开始编写代码，确保遵守以上所有规范。在编码过程中，如果遇到任何规范相关的问题，请参考以上原则和清单。
