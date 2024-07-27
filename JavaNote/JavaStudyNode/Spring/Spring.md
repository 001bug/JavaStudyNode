## Spring的基本介绍
### 基本介绍以及重要概念
概念:Spring是一个开源的轻量级框架.
![](assest/Pasted%20image%2020240723151919.png)
简单的解读,后面深入
1.IOC: 控制反转, 可以管理java 对象
2.AOP : 切面编程
3.JDBCTemplate : 是spring 提供一套访问数据库的技术, 其中声明式事务是基于ioc/aop 实现事务管理
4.相对于传统的开发模式,程序读取环境配置，然后自己创建对象.有着巨大的区别

#### IOC控制反转
 简单来说就是容器创建好对象，程序直接使用.![](assest/Pasted%20image%2020240723152901.png)
 1、Spring 根据配置文件xml/注解, 创建对象， 并放入到容(ConcurrentHashMap)中,并且可以完成对象之间的依赖.
 2、当需要使用某个对象实例的时候, 就直接从容器中获取即可
 3、程序员可以更加关注如何使用对象完成相应的业务, (以前是new ... ==> 注解/配置方式)

#### Spring的简单使用
.引入spring需要的依赖
![](assest/Pasted%20image%2020240723153502.png)
2.创建一个entity包,然后创建一个javabean(注意,一定要有无参构造器,不然反射会失效)
3.创建Spring配置文件,放在src的根路径下
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"  
       xmlns:util="http://www.springframework.org/schema/util"  
       xmlns:context="http://www.springframework.org/schema/context"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd  http://www.springframework.org/schema/util  
        http://www.springframework.org/schema/util/spring-util.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
	<bean class="com.hspedu.spring.bean.Monster" id="monster01">  
	    <property name="monsterId" value="100"/>  
	    <property name="name" value="牛魔王"/>  
	    <property name="skill" value="芭蕉扇"/>  
	</bean>
</beans>
```
配置文件中bean表示一个对象实例,class里面要填写 , 第二步中的属性,通过反射实现 , name是字段名,**id属性表示该java对象在spring容器中的id, 通过id可以获取到对象**
4.使用ioc容器得到java对象实例
```java
public void setProByDependencyInjection() {  
    ApplicationContext ioc =  
            new ClassPathXmlApplicationContext("配置文件");  
    类名 变量名 = ioc.getBean("配置信息中的id", 类名);  
    //使用对象的方法
}
```
第一步要创建容器ApplicationContext,该容器和配置文件是一对一的关系

**基于注解的快速使用**
组件注解的形式主要有
1. @Component 表示当前注解标识的是一个组件(常用)
2. @Controller 表示当前注解标识的是一个控制器，通常用于Servlet
3. @Service 表示当前注解标识的是一个处理业务逻辑的类，通常用于Service 类
4. @Repository 表示当前注解标识的是一个持久化层的类，通常用于Dao 类
```java
@Component
public class MyComponent{}
```
通常是一个组件
```java
@Controller
public class UserAction{}
```
通常是个servlet,但是在springmvc被另一种注解代替
```java
@Service
public class UserService{}
```
```java
@Repository
public class UserDao{
}
```
通常标记在持久化层
2.配置beans.xml
```xml
<!--配置自动扫描的包,注意需要加入context名称空间-->
<context:component-scan base-package="全类名"/>
```
要在benas配置文件中去配置
3.使用
```java
public void setProByDependencyInjection() {  
    ApplicationContext ioc =  
            new ClassPathXmlApplicationContext("配置文件");  
    类名 变量名 = ioc.getBean("配置信息中的id", 类名);  
    //使用对象的方法
}
```
**级联属性的配置使用**\
比如一个类的其中的属性是另一个类
```java
public class Dept{
	String name;
}
----------------------
public class Emp{
	Spring name;
	Dept dept;
}
```
然后再beans.xml中配置一下
```xml
<bean class="全类名.Dept" id="dept"/>
<bean calss="全类名.Emp" id="emp"/>
	<property name="name" value="jack"/>
	<property name="dept" ref="dept"/>
	<!--这里可以指定一下dept中的属性-->
	<property name="dept.name" value="开发部门"/>
</bean>
```
解释,property name="dept"会去引用上面的配好的,但是实例会使用下面自己定义的`<property name="dept.name" value="开发部门"/>`底层是调用set方法
### Spring原生容器底层结构
debug图示
主要是debug , ioc容器中里面的信息
![](assest/Pasted%20image%2020240723165559.png)
![](assest/Pasted%20image%2020240723165715.png)
![](assest/Pasted%20image%2020240723165727.png)
结论:在ioc中有个beanFactory工厂,他是实例化bean的,beanFactory里有很多的属性其中beanDefinitioMap里用ConcurrentHashMap集合形式存放了bean.xml的两个Monster,key-bean的id,val-bean的对象信息, 然后beanFactory里面的SigletonObjects也有ConcurrentHashMap集合,里面存放了初始化的单例对象
![](assest/Pasted%20image%2020240723212224.png)
Spring的底层设计有个小细节, Spring为了让程序员能快速看到单例,设计了一个名为elementData的属性,里面能快速查看出在容器内的实例id
## Spring的AOP
### AOP的快速使用和基本概念
##### 快速入门
1.引入包
![](assest/Pasted%20image%2020240724091918.png)
还要引入spring-aspects
2.编写一个接口SmartAnimal,其中getSum和getSub是目标方法
```java
public interface SmartAnimalable {  
    float getSum(float i, float j);   
    float getSub(float i, float j);  
}
```
3.然后实现接口去实现这些方法,注意,不需要手动实现,靠ioc控制
```java
@Component
public class SmartDog implements SmartAnimalable{}
```
4.实现切面类
```java
@Aspect//表示这是一个切面类
@Component//注入容器中
public class SmartAnimalAspect {
	@Before(value = "execution(public 返回类型 全类名.getSum(float, float))")  
public void showBeginLog(JoinPoint joinPoint) {  
    //通过连接点对象joinPoint 可以获取方法签名(获取目标方法的信息)  
    Signature signature = joinPoint.getSignature();  
}
}
```
主要分为两步,写注释的参数(要切到哪里),方法参数改为JoinPoin,可以通过JoinPoin获取目标方法信息
##### 基本概念
面向切面编程(AOP)是 Spring 框架的一个核心特性,AOP 允许程序员在不改变代码逻辑的情况下，通过声明方式向程序添加横切关注点
主要概念有**1.Aspect(切面) 2.Join Point(连接点) 3.Advice（通知）4.Pointcut（切点）**
![](assest/Pasted%20image%2020240724085323.png)
在传统的oop编程中一个方法只能在同类中起作用,而aop编程可以创造一个切面类,这个类中的方法可以作用在其他类中,这个其他类的方法调用要是动态代理实现的
![](assest/Pasted%20image%2020240724090545.png)
解读:
1.showBeginLog()方法可以理解为切入方法,类似于AOP.before()
2.getSum()方法是我们的目标方法,真正要执行的方法
3.@Before表示要包showBeginLog()切入到getSum()前执行
4.JoinPoint是连接点,通过JoinPoint可以得到目标方法