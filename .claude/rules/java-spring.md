# Java Spring Boot Coding Standards

## Technology Stack

- Java 17+
- Spring Boot 3.x
- MyBatis
- Redis
- MySQL

---

## Code Style

### Naming Conventions

- **Classes/Interfaces**: `PascalCase` (UserService, OrderController)
- **Methods/Fields**: `camelCase` (getUserById, createOrder)
- **Constants**: `SCREAMING_SNAKE_CASE` (MAX_RETRY_COUNT)
- **Packages**: lowercase (com.example.app.service)

### File Organization

```
src/main/java/com/example/app/
├── controller/     # REST controllers
├── service/        # Business logic
│   └── impl/       # Service implementations
├── mapper/         # MyBatis mappers
├── entity/         # Database entities
├── dto/            # Data transfer objects
├── vo/             # View objects
├── config/         # Configuration classes
├── exception/      # Custom exceptions
└── util/           # Utility classes
```

### Code Quality

- Methods: < 50 lines
- Classes: < 500 lines
- Cyclomatic complexity: < 10
- Nesting depth: < 4 levels

---

## Immutability

**CRITICAL: Always create new objects, never mutate existing ones**

```java
// WRONG - Mutation
public void updateUser(User user, String name) {
    user.setName(name);  // MUTATION!
}

// CORRECT - Immutability
public User updateUser(User user, String name) {
    return User.builder()
        .id(user.getId())
        .name(name)
        .email(user.getEmail())
        .build();
}
```

Use `final` fields by default. Use records for DTOs (Java 16+).

---

## Spring Boot Patterns

### Controller Layer

```java
@RestController
@RequestMapping("/api/users")
@Slf4j
public class UserController {

    private final UserService userService;

    // ✅ Constructor injection (not @Autowired)
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public Result<UserVO> getUser(@PathVariable Long id) {
        return Result.success(userService.getUserById(id));
    }

    @PostMapping
    public Result<Void> createUser(@Valid @RequestBody UserDTO dto) {
        userService.createUser(dto);
        return Result.success();
    }
}
```

**Rules:**
- ❌ No business logic in controllers
- ❌ No @Autowired field injection
- ✅ Use constructor injection
- ✅ Validate input with @Valid

### Service Layer

```java
@Service
@Slf4j
public class UserServiceImpl implements UserService {

    private final UserMapper userMapper;
    private final RedisTemplate<String, Object> redisTemplate;

    public UserServiceImpl(UserMapper userMapper,
                          RedisTemplate<String, Object> redisTemplate) {
        this.userMapper = userMapper;
        this.redisTemplate = redisTemplate;
    }

    @Transactional(rollbackFor = Exception.class)
    @Override
    public void createUser(UserDTO dto) {
        // Business logic here
        UserEntity entity = convertToEntity(dto);
        userMapper.insert(entity);

        // Cache
        String key = "user:info:" + entity.getId();
        redisTemplate.opsForValue().set(key, entity, 30, TimeUnit.MINUTES);
    }
}
```

**Rules:**
- ✅ Interface + implementation separation
- ✅ @Transactional with rollbackFor = Exception.class
- ✅ Constructor injection
- ❌ No database access in controllers

---

## MyBatis

### Mapper Interface

```java
@Mapper
public interface UserMapper {
    UserEntity selectById(Long id);
    List<UserEntity> selectByCondition(UserQuery query);
    int insert(UserEntity entity);
    int updateById(UserEntity entity);
    int deleteById(Long id);
}
```

### XML Rules

- ✅ Use `#{}` for parameters (prevents SQL injection)
- ❌ Never use `${}` with user input
- ❌ No `SELECT *` - list specific columns
- ✅ Use `<resultMap>` for complex mappings
- ✅ Use PageHelper for pagination

---

## Redis

### Key Naming Convention

```
Format: {module}:{function}:{id}
Examples:
  user:info:123
  order:cache:456
  lock:payment:789
```

### Usage Pattern

```java
// ✅ Always set expiration time
public void cacheUser(Long userId, UserVO user) {
    String key = "user:info:" + userId;
    redisTemplate.opsForValue().set(key, user, 30, TimeUnit.MINUTES);
}

// ✅ Distributed lock
public void processWithLock(String lockKey) {
    Boolean locked = redisTemplate.opsForValue()
        .setIfAbsent(lockKey, "1", 10, TimeUnit.SECONDS);
    if (Boolean.TRUE.equals(locked)) {
        try {
            // Business logic
        } finally {
            redisTemplate.delete(lockKey);
        }
    }
}
```

**Rules:**
- ✅ All cache keys must have expiration time
- ✅ Use consistent naming convention
- ❌ No data > 1MB in Redis

---

## Database

### Entity Class

```java
@Data
@TableName("t_user")
public class UserEntity {

    @TableId(type = IdType.AUTO)
    private Long id;

    private String username;
    private String email;

    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;
}
```

**Rules:**
- ✅ Table names use `t_` prefix
- ✅ Primary key: `id` (BIGINT)
- ✅ Must have `create_time` and `update_time`
- ✅ Soft delete with `deleted` field
- ❌ No DDL in code - use Flyway/Liquibase

---

## Error Handling

### Global Exception Handler

```java
@RestControllerAdvice
@Slf4j
public class GlobalExceptionHandler {

    @ExceptionHandler(BusinessException.class)
    public Result<Void> handleBusinessException(BusinessException e) {
        log.warn("Business exception: {}", e.getMessage());
        return Result.fail(e.getCode(), e.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public Result<Void> handleException(Exception e) {
        log.error("System exception", e);
        return Result.fail("System error, please try again");
    }
}
```

**Rules:**
- ✅ Use custom business exceptions
- ✅ Log all exceptions
- ❌ Never swallow exceptions
- ✅ Don't expose internal errors to users

---

## Logging

### Log Levels

- **ERROR**: System errors requiring immediate attention
- **WARN**: Business exceptions to monitor
- **INFO**: Key business flows
- **DEBUG**: Debug information (disabled in production)

### Usage

```java
@Slf4j
public class UserService {

    public void createUser(UserDTO dto) {
        log.info("Creating user, username=", dto.getUsername());

        try {
            // Business logic
            log.info("User created successfully, userId={}", userId);
        } catch (Exception e) {
            log.error("Failed to create user, username={}", dto.getUsername(), e);
            throw e;
        }
    }
}
```

**Rules:**
- ✅ Use @Slf4j annotation
- ✅ Use placeholders `{}`, not string concatenation
- ✅ Log key business operations
- ❌ No logging in loops
- ❌ No sensitive data (passwords, IDs)

---

## Security

### Input Validation

```java
@PostMapping
public Result<Void> createUser(@Valid @RequestBody UserDTO dto) {
    // @Valid automatically validates
}

@Data
public class UserDTO {
    @NotBlank(message = "Username cannot be empty")
    @Size(min = 3, max = 20, message = "Username length: 3-20")
    private String username;

    @Email(message = "Invalid email format")
    private String email;
}
```

**Rules:**
- ✅ Validate all user input
- ✅ Use @Valid + Bean Validation
- ✅ Use parameterized queries (MyBatis `#{}`)
- ❌ Never hardcode secrets
- ✅ Use environment variables for sensitive config

---

## Testing

### Unit Test Pattern

```java
@SpringBootTest
class UserServiceTest {

    @Autowired
    private UserService userService;

    @MockBean
    private UserMapper userMapper;

    @Test
    @DisplayName("Create user successfully")
    void testCreateUser() {
        UserDTO dto = new UserDTO();
        dto.setUsername("test");
        dto.setEmail("test@example.com");

        assertDoesNotThrow(() -> userService.createUser(dto));

        verify(userMapper).insert(any(UserEntity.class));
    }
}
```

**Rules:**
- ✅ Test coverage ≥ 80%
- ✅ Service layer must have unit tests
- ✅ Use JUnit 5 + Mockito
- ✅ Test naming: test + MethodName + Scenario
