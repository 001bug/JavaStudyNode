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
1.自动配置的基本介绍
在ssm中,需要配置Tomcat,、配置SpringMVC、配置如何扫描包、配置字符过滤器、配置视图解析器、文件上传等. 非常的麻烦 , 在SpringBoot中 , 能够自动配置 
2.SpringBoot自动配置了Tomcat,SpringMVC,字符过滤器等.具体如下验证
```java
public class MainApp {  
    public static void main(String[] args) {  
        ConfigurableApplicationContext ioc =  
                SpringApplication.run(MainApp.class, args);  
//查看容器里面的组件  
        String[] beanDefinitionNames = ioc.getBeanDefinitionNames();  
        for (String beanDefinitionName : beanDefinitionNames) {  
            System.out.println(beanDefinitionName);  
        }  
    }  
}
```
然后仔细能发现到characterEncodingFilter这个字符过滤器
第二种方式验证(debug)
![](assest/Pasted%20image%2020240731001004.png)
3.修改自动配置
	1.SpringBoot还会扫描特定的包 , 不需要像Spring那样要手动在web.xml中配置
	https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.structuring-your-code.using-the-default-package 这里可以查看SpringBoot到底扫描哪些包
	自定义扫包 , 增加扫描的包比如(指定的业务类不在特定的包下)
	![](assest/Pasted%20image%2020240731145444.png)
	就是在MainApp类下填写注解@SpringBootApplication(scanBasePackages="com.xxx") 这个scanBasePackages指定的是字符串数组
	---
	2.修改配置文件来更改生产环境
	SpringBoot项目中最重要最核心的配置文件是application.properties,所有框架的配置都可以在这个[配置文件](application)中说明,最终会映射到对应的类中(光标对应属性,然后ctrl+b就能找到,或者通过ioc容器找到)
	查看地址 https://blog.csdn.net/pbrlovejava/article/details/82659702
	3.自定义配置: **application.properties**,增加在springboot中的key-value值,然后应用在bean中
```java
@Controller  
public class HiController {  
    //需求,这里的website的值要从配置文件中获取  
	 @Value("${my.website}")
     private String website;  
       
     @RequestMapping("/hi")  
     @ResponseBody  
     public String hi() {  
         System.out.println("website-" + website);  
         return "hi~, SpringBoot";  
     }  
}
```
在application.properties中添加my.website=https://ww.baidu.com

4.SpringBoot在哪配置和自动配置遵守的加载原则
	![](assest/Pasted%20image%2020240731161952.png)
	根据源码可知,SpringBoot在类路径,类路径的config文件下,还有application等等
	2. 自动配置遵守按需加载原则:引入了哪个场景starter 就会加载该场景关联的jar 包，没有引入的starter 则不会加载其关联jar
	SpringBoot所有的自动配置功能都在spring-boot-autoconfigure包里面
	![](assest/Pasted%20image%2020240731163024.png)
	有默认的约定,在SpringBoot 的自动配置包, 一般是XxxAutoConfiguration.java, 对应 XxxxProperties.java,
	![](assest/Pasted%20image%2020240731163148.png)
	**总的来说,就是bean的实例会对应一个相关的xxproperties.java  xxAutoConfiguration.java  applicaton.properties. Springboot帮我们实例的bean是有非常多的依赖组件,会去读取application.properties,然后xxproperties.java会关联到bean里面**
#### 容器功能
注意:Spring注入组件的注解 , 在SpringBoot中仍然有效
`@Component @Controller @Service @Repository`

**SpringBoot中的注解**
1. Configuration
==回顾==一下传统的Spring通过注解或者xml配置文件获取ioc
```xml
<bean id="monster03" class="com.hspedu.springboot.bean.Monster">  
    <property name="name" value="牛魔王~"></property>  
    <property name="age" value="5000"></property>  
    <property name="skill" value="芭蕉扇~"></property>  
    <property name="id" value="1000"></property>  
</bean>
```
然后写使用类
```java
public void setProByDependencyInjection() {  
    ApplicationContext ioc =  
            new ClassPathXmlApplicationContext("beans.xml");  
    Monster monster = ioc.getBean("配置信息中的id", Monster);  
    //使用对象的方法
}
```

SpringBoot通过@Configuration标识为配置类用来注入组件,被Configuration标识的类等价于配置文件
```java
@Configuration
public class BeanConfig {
	@Bean
	public Monster monster01() {
		return new Monster(100, "牛魔王", 500, "芭蕉扇");
	}
}
```
解释:@Bean是给容器添加组件, monster01是默认的方法名,也是组件的id,返回类型是组件类型, 默认是单例模式(==如果不想要单例模式,那就在Bean的同一个地方加上`@Scope("prototype")`==)的. 也可以自定义id`@Bean(name="id")`
使用MainApp.java,从配置文件/容器中获取bean
```java
public static void main(String[] args) {  
    ConfigurableApplicationContext ioc = SpringApplication.run(MainApp.class, args);  
    Monster monster01 = ioc.getBean("monster01", Monster.class);  
}
```
注意事项: 被@Configuration注解的配置类也是组件,也放在ioc容器中
扩展:使用proxyBeanMethods代理bean方法
示例代码
```java
@Configuration(proxyBeanMethods=false)
public class BeanConfig {
	@Bean
	public Monster monster01() {
		return new Monster(100, "牛魔王", 500, "芭蕉扇");
	}
}
//默认放到约定的包中
public class MainApp{
	public static void main(String[] args){
		ConfigurableApplicationContext ioc = SpringApplication.run(MainApp.class, args);
		BeanConfig beanConfig=ioc.getBean(BeanConfig.class);
		Monster monster02 = beanConfig.monster01();
	}
}
```
1.Full(proxyBeanMethods = true)、【保证每个@Bean 方法被调用多少次返回的组件都是单实例的, 是代理方式】
2.Lite(proxyBeanMethods = false)【每个@Bean 方法被调用多少次返回的组件都是新创建的, 是非代理方式】
3.如果组件有依赖必须使用Full模式(默认).  如果不需要组件依赖,则使用Lite模式(轻量级,需要的时候才加载)
注意:proxyBeanMethods是在调用@Bean方法的,而被@Bean注解的方法在BeanConfig中,因此需要先获取BeanConfig组件 , 然后再调用方法
