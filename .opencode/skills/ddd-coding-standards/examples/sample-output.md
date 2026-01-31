# DDD分层架构代码规范检查 - 示例输出

## 违规示例项目结构

```
src/main/java/com/example/
├── api/
│   └── controller/
│       └── UserController.java        ✅ 符合规范
├── application/
│   └── service/
│       └── UserApplicationService.java ✅ 符合规范
├── domain/
│   ├── entity/
│   │   └── User.java                  ❌ 违规：使用了@Entity注解
│   ├── valueobject/
│   │   └── UserId.java                ✅ 符合规范
│   └── repository/
│       └── UserRepository.java        ✅ 符合规范（接口）
└── infrastructure/
    └── repository/
        └── UserRepositoryImpl.java    ✅ 符合规范
```

## 问题代码示例

### ❌ 违规：领域层使用了持久化注解

**文件:** `com/example/domain/entity/User.java`

```java
package com.example.domain.entity;

import jakarta.persistence.Entity;        // ❌ 违规
import jakarta.persistence.Table;         // ❌ 违规
import jakarta.persistence.Id;            // ❌ 违规
import org.springframework.data.annotation.Transient; // ❌ 违规

@Entity  // ❌ 违规：领域层不应使用持久化注解
@Table(name = "users")  // ❌ 违规
public class User {
    @Id  // ❌ 违规
    private Long id;

    private String name;

    // ✅ 业务逻辑方法符合规范
    public void changeName(String newName) {
        if (newName == null || newName.isBlank()) {
            throw new IllegalArgumentException("Name cannot be blank");
        }
        this.name = newName;
    }
}
```

### ✅ 正确：领域层应该是纯净的

**修复后的代码:**

```java
package com.example.domain.entity;

// 不引入任何持久化框架依赖
// 只使用纯Java代码

public class User {
    private UserId id;        // 使用值对象而非Long
    private String name;

    // 领域逻辑
    public void changeName(String newName) {
        if (newName == null || newName.isBlank()) {
            throw new IllegalArgumentException("Name cannot be blank");
        }
        this.name = newName;
    }

    // 领域规则
    public boolean isActive() {
        return name != null && !name.isBlank();
    }
}
```

### ❌ 违规：领域层引用了应用层

**文件:** `com/example/domain/service/DomainServiceExample.java`

```java
package com.example.domain.service;

import com.example.application.service.UserApplicationService; // ❌ 违规

public class DomainServiceExample {
    private final UserApplicationService appService; // ❌ 违规

    // 领域层不应该引用应用层
}
```

### ✅ 正确：基础设施层实现仓储接口

**文件:** `com/example/infrastructure/repository/UserRepositoryImpl.java`

```java
package com.example.infrastructure.repository;

import com.example.domain.entity.User;
import com.example.domain.repository.UserRepository;
import org.springframework.stereotype.Repository;
import jakarta.persistence.EntityManager;

@Repository  // ✅ 基础设施层可以使用Spring注解
public class UserRepositoryImpl implements UserRepository {

    private final EntityManager em;

    public UserRepositoryImpl(EntityManager em) {
        this.em = em;
    }

    @Override
    public User save(User user) {
        // 实现持久化逻辑
        return user;
    }
}
```

## 检查报告示例

```markdown
# DDD分层架构代码规范检查报告

## 📁 项目信息
- 项目路径: /workspace/my-project
- 检查时间: 2024-01-23 10:30:00

## 🏗️ 分层结构分析

检测到以下分层结构：

```
com.example.api (接口层)
  └── controller (2个Controller)
  └── dto (5个DTO)

com.example.application (应用层)
  └── service (3个ApplicationService)
  └── command (2个Command)
  └── query (1个Query)

com.example.domain (领域层)
  └── entity (8个Entity)
  └── valueobject (3个ValueObject)
  └── service (2个DomainService)
  └── repository (5个Repository接口)

com.example.infrastructure (基础设施层)
  └── repository (5个RepositoryImpl)
  └── external (1个ExternalClient)
```

## ✅ 通过的检查

- ✅ 项目采用了清晰的DDD四层结构
- ✅ 接口层正确命名了Controller类
- ✅ 应用层正确命名了ApplicationService类
- ✅ 基础设施层正确实现了Repository接口
- ✅ 使用了值对象模式
- ✅ 定义了清晰的领域边界

## ⚠️ 发现的问题

### 严重问题（阻断性）

1. **领域层使用了持久化注解**
   - 位置: `com/example/domain/entity/User.java:3-5`
   - 问题描述:
     ```java
     import jakarta.persistence.Entity;
     import jakarta.persistence.Table;
     import jakarta.persistence.Id;
     @Entity
     @Table(name = "users")
     ```
   - 违反原则: 领域层独立性原则 - 领域层不应依赖任何持久化框架
   - 建议修复:
     1. 将@Entity等注解移至基础设施层
     2. 在infrastructure层创建JpaUser作为持久化模型
     3. 使用Repository模式在infrastructure层进行领域模型与持久化模型的转换

2. **领域层引用了Spring框架注解**
   - 位置: `com/example/domain/entity/Order.java:7`
   - 问题描述:
     ```java
     import org.springframework.data.annotation.Transient;
     ```
   - 违反原则: 领域层独立性原则
   - 建议修复: 使用Java的transient关键字替代

### 警告问题

1. **建议使用值对象而非基本类型**
   - 位置: `com/example/domain/entity/User.java:15`
   - 问题描述:
     ```java
     private Long id;  // 使用了基本类型Long
     ```
   - 建议改进:
     ```java
     private UserId id;  // 使用值对象包装ID
     ```

2. **DTO命名建议更具体**
   - 位置: `com/example/api/dto/UserData.java`
   - 问题描述: 类名"UserData"不够明确
   - 建议改进: 重命名为`UserResponse`或`UserDTO`以清晰表达其用途

3. **建议引入Command/Query模式**
   - 位置: 应用层
   - 问题描述: ApplicationService方法参数过多，建议封装为Command/Query对象
   - 建议改进:
     ```java
     // 当前
     void createUser(String name, String email, int age);

     // 建议
     void createUser(CreateUserCommand command);
     ```

## 📊 评分统计

| 维度 | 得分 | 说明 |
|------|------|------|
| 分层规范性 | 90/100 | 结构清晰，符合DDD分层 |
| 领域层独立性 | 60/100 | 存在持久化框架依赖，需要改进 |
| 命名规范性 | 85/100 | 大部分命名规范，少数可优化 |
| 依赖方向 | 95/100 | 依赖方向基本正确 |
| **总体评分** | **82/100** | 良好，需改进领域层独立性 |

## 📝 改进建议

### 优先级1：修复领域层独立性（关键）

1. **移除领域层的持久化依赖**
   - 创建独立的领域模型，不使用任何JPA注解
   - 在infrastructure层创建对应的JPA实体
   - 实现领域模型与JPA实体的转换器

2. **重构User实体示例**
   ```java
   // domain/entity/User.java (纯净的领域模型)
   public class User {
       private final UserId id;
       private String name;
       private Email email;

       public User(UserId id, String name, Email email) {
           this.id = id;
           this.name = name;
           this.email = email;
       }

       // 领域行为
       public void changeName(String newName) {
           // 业务规则验证
           if (newName == null || newName.isBlank()) {
               throw new DomainException("User name cannot be blank");
           }
           this.name = newName;
       }
   }

   // infrastructure/persistence/JpaUser.java (持久化模型)
   @Entity
   @Table(name = "users")
   public class JpaUser {
       @Id
       private Long id;
       private String name;
       private String email;
       // 持久化相关注解和逻辑
   }

   // infrastructure/persistence/UserMapper.java
   public class UserMapper {
       public static User toDomain(JpaUser jpaUser) {
           // 转换逻辑
       }

       public static JpaUser toJpa(User user) {
           // 转换逻辑
       }
   }
   ```

### 优先级2：引入值对象

- 创建`UserId`、`Email`等值对象
- 用值对象替代基本类型，增强类型安全性
- 将相关逻辑封装到值对象中

### 优先级3：优化应用层

- 引入CQRS模式
- 使用Command/Query对象封装参数
- 简化ApplicationService接口

## 🎯 总结

项目的DDD分层架构基础良好，主要问题集中在**领域层独立性**方面。建议优先解决领域层的持久化依赖问题，这是DDD的核心原则。修复后可获得更清晰的分层架构和更好的可维护性。
```
