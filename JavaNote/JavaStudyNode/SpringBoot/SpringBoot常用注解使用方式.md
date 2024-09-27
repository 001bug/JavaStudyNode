## @RequestMapping
在 Spring Boot 中，`@RequestMapping` 注解用于定义处理 HTTP 请求的路由。它可以在控制器类和方法上使用，支持多种 HTTP 方法和请求参数，提供灵活的请求映射功能。
### 1. **基本用法**
`@RequestMapping` 可以用于类级别和方法级别：
- **类级别**：定义一个基础的 URL 路径。
- **方法级别**：定义具体的操作路径。
#### 示例：
```java
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class MyController {

    @RequestMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```
在上面的例子中，访问 `/api/hello` 时，会调用 `hello()` 方法。
### 2. **指定 HTTP 方法**
可以通过 `method` 属性指定支持的 HTTP 方法（GET、POST、PUT、DELETE 等）。
#### 示例：
```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class MyController {

    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public String hello() {
        return "Hello, World!";
    }
    @RequestMapping(value = "/hello", method = RequestMethod.POST)
    public String helloPost(@RequestBody String name) {
        return "Hello, " + name + "!";
    }
}
```
### 3. **简化的 HTTP 方法注解**
Spring 还提供了一些简化的注解，作为 `@RequestMapping` 的替代，用于特定的 HTTP 方法：
- **`@GetMapping`**：处理 GET 请求
- **`@PostMapping`**：处理 POST 请求
- **`@PutMapping`**：处理 PUT 请求
- **`@DeleteMapping`**：处理 DELETE 请求
#### 示例：
```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }

    @PostMapping("/hello")
    public String helloPost(@RequestBody String name) {
        return "Hello, " + name + "!";
    }
}
```
### 4. **路径变量和请求参数**
可以使用路径变量和请求参数来获取动态值。
#### 示例：
```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/hello/{name}")
    public String hello(@PathVariable String name) {
        return "Hello, " + name + "!";
    }

    @GetMapping("/hello")
    public String helloWithParam(@RequestParam String name) {
        return "Hello, " + name + "!";
    }
}
```
- **`@PathVariable`** 用于获取 URL 路径中的变量。
- **`@RequestParam`** 用于获取请求参数。
### 5. **请求头和请求体**
可以通过 `@RequestHeader` 和 `@RequestBody` 获取请求头和请求体。
#### 示例：
```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class MyController {

    @PostMapping("/hello")
    public String helloWithHeader(@RequestHeader("User-Agent") String userAgent, @RequestBody String name) {
        return "Hello, " + name + "! Your user agent is: " + userAgent;
    }
}
```
### 6. **返回类型和状态码**
可以指定返回类型和状态码，使用 `ResponseEntity`：
#### 示例：
```java
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class MyController {

    @GetMapping("/hello")
    public ResponseEntity<String> hello() {
        return ResponseEntity.ok("Hello, World!"); // 200 OK
    }

    @PostMapping("/hello")
    public ResponseEntity<String> helloPost(@RequestBody String name) {
        return ResponseEntity.status(201).body("Hello, " + name + "!"); // 201 Created
    }
}
```
### 总结
`@RequestMapping` 是 Spring Boot 中处理 HTTP 请求的重要注解，支持灵活的请求映射和多种请求参数的处理。通过使用类级别和方法级别的注解组合，可以构建出功能丰富的 RESTful API。
## @ConfigurationProperties
在 Spring Boot 中，`@ConfigurationProperties` 是一个非常常用的注解，主要用于将外部配置文件（如 `application.properties` 或 `application.yml`）中的属性映射到 Java 对象中。这种方式提供了一种强类型的配置管理，简化了复杂的配置处理流程。
### `@ConfigurationProperties` 的作用
`@ConfigurationProperties` 的作用是将配置文件中的属性和 Java 类的字段自动映射并注入。通过这种方式，开发人员可以方便地管理和使用应用程序的配置属性。
### `@ConfigurationProperties` 的使用步骤
1. **创建配置类**：定义一个 Java 类并使用 `@ConfigurationProperties` 注解来声明这个类对应某一部分配置属性。
2. **在配置文件中声明属性**：在 `application.properties` 或 `application.yml` 文件中声明相应的配置项。
3. **启用配置属性绑定**：使用 `@EnableConfigurationProperties` 或在类上加上 `@Component` 使配置类被 Spring 管理。
### 使用示例
#### 1. 创建配置类
假设我们需要读取应用程序的数据库配置：
```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "database") // 指定配置前缀
public class DatabaseProperties {

    private String url;
    private String username;
    private String password;

    // getters and setters

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```
- `@ConfigurationProperties(prefix = "database")` 表示这个类会绑定以 `database` 开头的配置项。
#### 2. 在 `application.properties` 文件中声明配置属性
```properties
database.url=jdbc:mysql://localhost:3306/mydb
database.username=root
database.password=secret
```
#### 3. 在应用中使用配置属性

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class DatabaseController {

    @Autowired
    private DatabaseProperties databaseProperties;

    @GetMapping("/dbinfo")
    public String getDatabaseInfo() {
        return "Database URL: " + databaseProperties.getUrl() +
               ", Username: " + databaseProperties.getUsername();
    }
}
```

通过 `@Autowired` 注入 `DatabaseProperties` 类，你可以轻松获取配置文件中的数据库连接信息。
### 作用

- **集中配置管理**：将应用程序配置项与代码分离，可以灵活地修改应用配置，而不需要修改代码。特别适合管理复杂的应用程序配置。
- **类型安全**：将配置项映射到强类型的 Java 类中，避免了通过 `Environment` 或 `@Value` 方式获取配置时出现的拼写错误和类型转换问题。
- **易于维护**：当配置项变得复杂时，可以通过 `@ConfigurationProperties` 分层管理，避免将所有配置都放在一个类或一个文件中。
### 需要注意的点

1. **Getter 和 Setter 方法**：Java 类的属性需要有对应的 getter 和 setter 方法，Spring 才能完成注入。
2. **嵌套配置**：`@ConfigurationProperties` 支持嵌套配置，例如，类中的字段也可以是一个对象，对象中的字段会进一步映射配置文件中的属性。
#### 嵌套配置示例
```java
@Component
@ConfigurationProperties(prefix = "app")
public class AppProperties {

    private final Security security = new Security();

    public Security getSecurity() {
        return security;
    }

    public static class Security {
        private boolean enabled;
        private String secretKey;

        public boolean isEnabled() {
            return enabled;
        }

        public void setEnabled(boolean enabled) {
            this.enabled = enabled;
        }

        public String getSecretKey() {
            return secretKey;
        }

        public void setSecretKey(String secretKey) {
            this.secretKey = secretKey;
        }
    }
}
```
在 `application.properties` 中配置嵌套属性：
```properties
app.security.enabled=true
app.security.secretKey=mySecretKey
```
### 与 `@Value` 的对比

`@Value` 也可以用于读取配置文件中的属性，但 `@ConfigurationProperties` 更适合用于管理大量和复杂的配置项，尤其是涉及嵌套配置时。

- `@Value` 是一种单个属性注入的方式，适合读取少量简单的属性。
- `@ConfigurationProperties` 支持类型安全的批量注入，非常适合复杂结构的配置。
### 总结

`@ConfigurationProperties` 是 Spring Boot 中用于管理和绑定配置属性的强大工具。它能够使配置更加清晰，便于维护，避免错误，并且具有良好的可扩展性。在大型项目中使用 `@ConfigurationProperties` 能提高配置管理的效率和可维护性。
## @Autowired
### `@Autowired`的作用和使用场景
**1.自动依赖注入**:`@Autowired` 的核心作用是自动将 Spring 容器中的某个 Bean 注入到需要该 Bean 的类中。开发者只需要在字段、构造方法或 setter 方法上使用 `@Autowired` 注解，Spring 就会自动找到并注入相应的 Bean。
**2.依赖注入的场景**:
* 当一个类依赖另一个类时,传统方式可能通过构造方法或setter方法手动注入依赖对象.
* `@Autowired`通过依赖注入的方式, 简化了对象创建和管理, 使开发者可以专注业务逻辑, 而不是关心对象的创建和生命周期的管理
### `@Autowired`的使用方式
**1.在字段上使用(最常用)**
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```
在上面的例子中，`UserService` 类依赖于 `UserRepository`，Spring 会自动将 `UserRepository` 的实例注入到 `UserService` 中。
**2.在方法上使用**
```java
@Service
public class UserService {
    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```
使用构造方法注入时，`@Autowired` 可以省略，Spring 会自动注入构造方法的参数。构造方法注入的好处是可以让字段为 `final`，并且有助于单元测试中的依赖注入。