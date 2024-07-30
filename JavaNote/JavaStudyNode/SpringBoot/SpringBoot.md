## SpringBoot的概念
**SpringBoot的官方文档**
官网: https://spring.io/projects/spring-boot
学习文档: https://docs.spring.io/spring-boot/docs/current/reference/html/
在线API: https://docs.spring.io/spring-boot/docs/current/api/

**什么是SpringBoot**
Spring Boot 可以轻松创建独立的、生产级的基于Spring 的应用程序 , pring Boot 直接嵌入Tomcat、Jetty 或Undertow ，可以"直接运行" SpringBoot 应用程序

**Spring SpringMVC SpringBoot的关系**
大概关系是:Spring Boot > Spring > Spring MVC(组件关系)
* Spring的核心是IOC和AOP,IOC提供了依赖注入的容器,AOP提供面向切面编程的特性,Spring接管了Web层,业务层,DAO层,持久性层的组件,并配置各种bean和维护bean相关的关系
* Spring MVC 只是Spring 处理WEB 层请求的一个模块/组件, Spring MVC 的基石是
  Servlet
* SpringBoot是为了简化开发,推出的封神框架(约定优于配置`[COC]`),简化了Spring项目的配置流程.**无需过多关注XML的配置**

**约定优于配置**

## SpringBoot的快速入门
首先要确定,开发环境是jdk8以上, maven在3.5+
1.创建maven项目,引入web项目场景启动器
```xml
<parent>  
  <groupId>org.springframework.boot</groupId>  
  <artifactId>spring-boot-starter-parent</artifactId>  
  <version>2.5.3</version>  
</parent>  
<!-- 导入web 项目场景启动器,会自动导入和web 开发相关依赖,非常方便-->  
<dependencies>  
  <dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-web</artifactId>  
  </dependency>  
</dependencies>
```
web项目场景启动器,会自动导入web开发相关的依赖.
打开maven关系图
点击一个类或者快捷键`Ctrl+Alt+Shift+U`
![](assest/Pasted%20image%2020240730191326.png)
2.创建SpringBoot应用主程序
```java
@SpringBootApplication  
public class MainApp {  
    public static void main(String[] args) {  
//启动SpringBoot 应用程序  
        SpringApplication.run(MainApp.class, args);  
    }  
}
```
3.创建控制器
```java
@Controller  
public class HelloController {  
    @RequestMapping("/hello")  
    @ResponseBody  
    public String hello(){  
        return "hello, spring boot";  
    }  
}
```
通过快速入门,SpringBoot比传统的SSM开发,简化整合了很多步骤,提高了开发效率
## SpringBoot的功能
#### 依赖管理
* 依赖管理
	1.技术背景:在快速入门中,原本ssm繁杂的jar包引入在springboot中只需要要引入**父项目**和web场景启动器就完成的所有相关包的引入. 这种功能的实现就靠的是**依赖管理**的实现
	spring-boot-starter-parent 还有父项目(对着前面的id,按住ctrl点击parent), 声明了开发中常用的依赖的版本号
	![](assest/Pasted%20image%2020240730200549.png)
	2.能够进行自动版本仲裁,没有指定某个jar的版本,会以父项目指定的版本为主
	![](assest/Pasted%20image%2020240730200844.png)
	3.可以自己指定某个jar包的版本
	可以是在pom.xml中指定
	![](assest/Pasted%20image%2020240730201850.png)
	可以在父类spring-boot-dependencies.pom中改写
	![](assest/Pasted%20image%2020240730201928.png)
#### starter场景启动器
1.概念: starter是一个大集合 , 开发中引入了相关场景的starter,那么这个场景中所有的相关依赖都引入进来了. 引入web-starter,就会引入所有web相关的包

2.依赖树![](assest/Pasted%20image%2020240730204437.png)

3.第三方starter
第三方starter 不要从spring-boot 开始，因为这是官方spring-boot 保留的命名方式的。
第三方启动程序通常以项目名称开头。例如，名为thirdpartyproject 的第三方启动程序项
目通常被命名为thirdpartyproject-spring-boot-starter。xxx-spring-boot-starter 是第三方为我们提供的简化开发的场景启动器
#### 自动配置


#### 容器功能
