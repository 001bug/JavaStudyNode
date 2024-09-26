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