# Java + SpringBoot DDD 代码规范

## 技术栈
- 语言：Java
- 框架：SpringBoot
- 构建工具：Gradle

## 面向对象编程规范

### 组合优于继承
- 优先使用组合而非继承来实现代码复用
- 继承仅用于 "is-a" 关系，组合用于 "has-a" 关系

```java
// ✅ 推荐：使用组合
class OrderService {
    private final PaymentProcessor paymentProcessor;
    private final InventoryValidator inventoryValidator;
}

// ❌ 避免：过度继承
class OrderService extends PaymentProcessor { }
```

### 接口命名约定
- **形容词风格**：表示能力或行为，如 `Runnable`, `Callable`, `Validatable`, `Processable`
- **名词风格**：表示角色或职责，如 `Repository`, `Factory`, `Builder`, `Strategy`

```java
// 形容词风格 - 表示能力
public interface Validatable {
    ValidationResult validate();
}

// 名词风格 - 表示角色
public interface UserRepository {
    void save(User user);
}
```

### 状态变化方法返回新对象（支持链式调用）
- 修改状态的方法应返回当前对象，支持链式调用
- 避免返回 void 的状态修改方法（除了明确的 setter）

```java
// ✅ 推荐：支持链式调用
public class Order {
    public Order addItem(OrderItem item) {
        this.items.add(item);
        return this;
    }

    public Order applyDiscount(Discount discount) {
        this.discount = discount;
        return this;
    }
}

// 使用
order.addItem(item).applyDiscount(discount).submit();

// ❌ 避免：void 返回类型无法链式调用
public class Order {
    public void addItem(OrderItem item) {
        this.items.add(item);
    }
}
```

### 业务异常规范
- 自定义异常类继承自 `RuntimeException`
- 异常命名清晰表达业务意图
- 使用语义化的异常类而非通用的 `RuntimeException`

```java
// ✅ 推荐：自定义业务异常
public class InsufficientInventoryException extends RuntimeException {
    public InsufficientInventoryException(String productId, int requested, int available) {
        super(String.format("Insufficient inventory for product %s: requested %d, available %d",
                           productId, requested, available));
    }
}

public class InvalidEmailException extends RuntimeException {
    public InvalidEmailException(String email) {
        super(String.format("Invalid email format: %s", email));
    }
}

// 使用
if (inventory < requested) {
    throw new InsufficientInventoryException(productId, requested, inventory);
}

// ❌ 避免：通用异常
throw new RuntimeException("Not enough inventory");
throw new IllegalArgumentException("Invalid email");
```

### 异常层次结构
```
RuntimeException
├── DomainException (领域异常基类)
│   ├── BusinessRuleViolationException
│   ├── InvalidStateException
│   └── DomainNotFoundException
├── ApplicationException (应用异常基类)
│   ├── ValidationException
│   └── ConflictException
└── InfrastructureException (基础设施异常基类)
    ├── ExternalServiceException
    └── RepositoryException
```

## 自动化测试规范

### 测试框架与配置
- 使用 **JUnit 5** 作为测试框架
- 使用 `@SpringBootTest` 进行集成测试
- Controller 测试使用 `@WebMvcTest` 或 `@SpringBootTest`

```java
@SpringBootTest
@AutoConfigureMockMvc
class UserControllerTest {
    @Autowired
    private MockMvc mockMvc;
}
```

### 测试替身策略（核心原则）
- **系统边界组件**：调用第三方服务 API、消息队列、外部系统 - 使用测试替身（Mock）
- **系统内部组件**：所有内部服务、仓储 - 注入真实对象
- **数据库**：使用内存数据库 H2 进行测试

```java
@SpringBootTest
@AutoConfigureMockMvc
class OrderControllerTest {

    @Autowired
    private MockMvc mockMvc;

    // ✅ 真实对象：内部服务
    @Autowired
    private OrderApplicationService orderService;

    @Autowired
    private OrderRepository orderRepository;

    @Autowired
    private ProductRepository productRepository;

    // ✅ 测试替身：外部服务
    @MockBean
    private PaymentGatewayClient paymentGatewayClient;

    @MockBean
    private EmailServiceClient emailServiceClient;

    @Test
    void should_create_order_successfully_when_valid_request_given() {
        // 使用真实服务 + H2 内存数据库进行测试
    }
}
```

### 禁止事项
- **禁止使用 `@MockBean`**：对系统内部的任何组件使用 MockBean
- **禁止使用 `@Mock`**（Mockito）：对系统内部组件进行 Mock

```java
// ❌ 严格禁止
@MockBean
private OrderRepository orderRepository;  // 内部组件，应该用真实实现

// ❌ 严格禁止
@Mock
private OrderApplicationService orderService;  // 内部组件，应该用真实实现
```

### 测试方法命名规范
- 格式：`should***_when***_given***`
- **全部小写**，使用下划线分隔
- 清晰表达：期望结果 _ 当条件发生时 _ 给定前置条件

```java
// ✅ 正确的测试命名
@Test
void should_return_order_not_found_when_query_by_non_existent_id_given() {
    // given
    String nonExistentId = "non-existent-id";

    // when
    Throwable thrown = catchThrowable(() -> orderService.getOrder(nonExistentId));

    // then
    assertThat(thrown).isInstanceOf(OrderNotFoundException.class);
}

@Test
void should_create_order_successfully_when_valid_request_given() {
    // given
    CreateOrderRequest request = new CreateOrderRequest("product-123", 2);

    // when
    OrderResponse response = orderService.createOrder(request);

    // then
    assertThat(response.getId()).isNotNull();
    assertThat(response.getStatus()).isEqualTo(OrderStatus.PENDING);
}

@Test
void should_reject_order_when_insufficient_inventory_given() {
    // given
    CreateOrderRequest request = new CreateOrderRequest("product-123", 999);

    // when
    Throwable thrown = catchThrowable(() -> orderService.createOrder(request));

    // then
    assertThat(thrown).isInstanceOf(InsufficientInventoryException.class);
}
```

### 测试配置示例

```java
@SpringBootTest
@AutoConfigureMockMvc
@TestPropertySource(properties = {
    "spring.datasource.url=jdbc:h2:mem:testdb",
    "spring.jpa.hibernate.ddl-auto=create-drop"
})
class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    // 内部组件使用真实实现
    @Autowired
    private UserApplicationService userApplicationService;

    @Autowired
    private UserRepository userRepository;

    // 外部服务使用测试替身
    @MockBean
    private SMSServiceClient smsServiceClient;

    @MockBean
    private EmailServiceClient emailServiceClient;
}
```

## 分层依赖关系

```
      interface (接口层)
           ↓
      application (应用层)
           ↓
       domain (领域层)
           ↑
 infrastructure (基础设施层)
```

**依赖规则：**
- interface 层 → application 层（接口层依赖应用层）
- interface 层 ✗ domain 层（接口层不能直接依赖领域层）
- application 层 → domain 层（应用层依赖领域层）
- infrastructure 层 → domain 层（基础设施层依赖领域层）
- infrastructure 层 → application 层（基础设施层可以依赖应用层）
- infrastructure 层 → interface 层（基础设施层可以依赖接口层）
- domain 层：不依赖任何其他层，只能被依赖

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

或使用替代命名：

```
project-root/
├── src/main/java/
│   └── com/example/app/
│       ├── api/              # 接口层
│       ├── application/      # 应用层
│       ├── domain/           # 领域层
│       └── infrastructure/   # 基础设施层
```

## 命名规范

| 层         | 类名后缀/模式                                         | 说明           |
| ---------- | ----------------------------------------------------- | -------------- |
| 接口层     | `*Controller`, `*Resource`, `*Api`              | 处理HTTP请求   |
| 接口层     | `*Request`, `*Response`, `*DTO`                 | 数据传输对象   |
| 应用层     | `*ApplicationService`, `*AppService`, `*Facade` | 应用服务       |
| 应用层     | `*Command`, `*Query`                              | 命令查询对象   |
| 领域层     | `*Entity`, `*` (无后缀)                           | 聚合根/实体    |
| 领域层     | `*ValueObject`, `*VO`                             | 值对象         |
| 领域层     | `*DomainService`, `*Domain`                       | 领域服务       |
| 领域层     | `*Repository` (接口)                                | 仓储接口       |
| 基础设施层 | `*RepositoryImpl`, `*Dao`                         | 仓储实现       |
| 基础设施层 | `*Client`, `*Adapter`                             | 外部服务适配器 |

## 领域层独立性检查

领域层是DDD的核心，必须保持完全独立：

**检查领域层不应包含：**

- `import org.springframework.*;` (除注解外)
- `import org.springframework.web.*;` ❌
- `import org.springframework.stereotype.*;` (可接受)
- `import org.springframework.beans.factory.*;` ❌
- `import javax.persistence.*;` 或 `import jakarta.persistence.*;` ❌
- `import javax.validation.*;` ❌
- `import lombok.*;` (可接受，但不鼓励在领域模型中使用)
- `import com.fasterxml.jackson.*;` ❌
- `import com.example.app.application.*;` ❌ (不应依赖应用层)
- `import com.example.app.interface.*;` ❌ (不应依赖接口层)
- `import com.example.app.infrastructure.*;` ❌ (不应依赖基础设施层)

**检查领域层应该包含：**

- 纯Java业务逻辑
- 领域模型（Entity）
- 值对象（ValueObject）
- 领域服务（DomainService）
- 领域事件（DomainEvent）
- 仓储接口（Repository接口，非实现）

## 包引用违规检查命令

```bash
# 检查领域层是否引用了Spring Web
grep -r "import org.springframework.web" domain/

# 检查领域层是否引用了持久化注解
grep -r "import javax.persistence" domain/
grep -r "import jakarta.persistence" domain/
grep -r "@Entity\|@Table" domain/

# 检查领域层是否引用了应用层
grep -r "import.*application" domain/

# 检查领域层是否引用了接口层
grep -r "import.*interface" domain/
grep -r "import.*api.controller" domain/

# 检查领域层是否引用了基础设施层
grep -r "import.*infrastructure" domain/
```

## Gradle 依赖配置

典型的 SpringBoot + Gradle 项目依赖配置：

```gradle
dependencies {
    // Spring Boot 核心依赖
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    
    // 测试依赖
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'com.h2database:h2'
}
```

## Java 特定的 DDD 约定

### 使用 Lombok 简化实体定义

```java
import lombok.Data;

@Data
public class User {
    private String id;
    private String name;
    private String email;

    public void changeEmail(String newEmail) {
        this.email = newEmail;
    }
}
```

### 使用接口定义仓储

```java
public interface UserRepository {
    void save(User user);
    Optional<User> findById(String id);
}
```

### 使用 @Transactional 管理事务

```java
import org.springframework.transaction.annotation.Transactional;

@Service
@Transactional
public class UserService {
    private final UserRepository userRepository;

    public User createUser(CreateUserCommand command) {
        User user = new User(command.getName(), command.getEmail());
        userRepository.save(user);
        return user;
    }
}
```
