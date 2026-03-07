---
icon: simple/springboot
---

# SpringBoot 配置指南

:material-calendar: 2026-03-07 · :material-tag: Java, SpringBoot, 框架

---

Spring Boot 让 Spring 应用的搭建和开发变得极为简单。本文记录项目搭建和常用配置。

## 快速开始

### 创建项目

推荐使用 [Spring Initializr](https://start.spring.io/) 快速生成项目骨架，也可以通过 IDE 内置的向导创建。

核心依赖选择：

``` xml title="pom.xml 核心依赖"
<dependencies>
    <!-- Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- 数据库 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- 测试 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### 启动类

``` java title="Application.java"
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## 配置文件

Spring Boot 支持 `application.properties` 和 `application.yml` 两种格式。推荐使用 YAML，结构更清晰。

=== "application.yml"

    ``` yaml
    server:
      port: 8080

    spring:
      datasource:
        url: jdbc:mysql://localhost:3306/mydb
        username: root
        password: ${DB_PASSWORD}
        driver-class-name: com.mysql.cj.jdbc.Driver

      jpa:
        hibernate:
          ddl-auto: update
        show-sql: true
    ```

=== "application.properties"

    ``` properties
    server.port=8080

    spring.datasource.url=jdbc:mysql://localhost:3306/mydb
    spring.datasource.username=root
    spring.datasource.password=${DB_PASSWORD}
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

    spring.jpa.hibernate.ddl-auto=update
    spring.jpa.show-sql=true
    ```

!!! warning "安全提示"

    不要将数据库密码直接写在配置文件中。使用环境变量 `${DB_PASSWORD}` 或外部配置中心来管理敏感信息。

### 多环境配置

``` yaml title="application.yml — 多环境"
spring:
  profiles:
    active: dev

---
spring:
  config:
    activate:
      on-profile: dev
  datasource:
    url: jdbc:mysql://localhost:3306/dev_db

---
spring:
  config:
    activate:
      on-profile: prod
  datasource:
    url: jdbc:mysql://prod-server:3306/prod_db
```

## 常用注解

| 注解 | 用途 | 示例 |
|------|------|------|
| `@RestController` | RESTful 控制器 | 自动序列化返回值为 JSON |
| `@RequestMapping` | 请求映射 | `@GetMapping("/api/users")` |
| `@Autowired` | 依赖注入 | 自动注入 Bean |
| `@Value` | 注入配置值 | `@Value("${server.port}")` |
| `@Component` | 组件扫描 | 标记为 Spring Bean |
| `@Configuration` | 配置类 | 定义 Bean 的配置 |

## RESTful API 示例

``` java title="UserController.java" hl_lines="1-2"
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<User> list() {
        return userService.findAll();
    }

    @GetMapping("/{id}")
    public User getById(@PathVariable Long id) {
        return userService.findById(id);
    }

    @PostMapping
    public User create(@RequestBody User user) {
        return userService.save(user);
    }

    @DeleteMapping("/{id}")
    public void delete(@PathVariable Long id) {
        userService.deleteById(id);
    }
}
```

## 自动装配原理

Spring Boot 的核心魔法在于**自动装配**（Auto-Configuration）。

``` mermaid
graph LR
    A["@SpringBootApplication"] --> B["@EnableAutoConfiguration"]
    B --> C["spring.factories / AutoConfiguration.imports"]
    C --> D["条件注解过滤"]
    D --> E["自动注册 Bean"]
```

??? info "关键条件注解"

    | 注解 | 含义 |
    |------|------|
    | `@ConditionalOnClass` | 类路径存在指定类时生效 |
    | `@ConditionalOnMissingBean` | 容器中不存在指定 Bean 时生效 |
    | `@ConditionalOnProperty` | 配置属性满足条件时生效 |

    这意味着：你引入了 `spring-boot-starter-web`，Spring Boot 就自动配置好了 Tomcat、DispatcherServlet 等组件，无需手动配置。

## 总结

| 要点 | 说明 |
|------|------|
| 项目创建 | Spring Initializr 或 IDE 向导 |
| 配置管理 | 推荐 YAML，支持多环境 profile |
| 注解驱动 | `@RestController` + `@Autowired` 快速开发 |
| 自动装配 | 引入 starter 即自动配置，约定优于配置 |

Spring Boot 大幅降低了 Spring 应用的入门门槛，是 Java 后端开发的必备技能。:sparkles:
