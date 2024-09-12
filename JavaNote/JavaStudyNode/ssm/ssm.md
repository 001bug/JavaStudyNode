## 后端环境搭建
首先,ssm是Spring,Spring MVC和MyBatis这三个框架的组合,所以要搭建适合这三个框架的开发环境
1.引入相应的包,这里要注意避免依赖冲突
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.ohmygod</groupId>  
  <artifactId>jiajumail3</artifactId>  
  <packaging>war</packaging>  
  <version>1.0-SNAPSHOT</version>  
  <name>jiajumail3 Maven Webapp</name>  
  <url>http://maven.apache.org</url>  
  <!--集中定义版本号-->  
  <properties>  
    <spring.version>5.0.5.RELEASE</spring.version>  
    <aspectjweaver.version>1.8.7</aspectjweaver.version>  
    <servlet-api.version>2.5</servlet-api.version>  
    <mybatis.version>3.4.6</mybatis.version>  
    <mybatis-spring.version>1.3.1</mybatis-spring.version>  
    <mysql-connector-java.version>8.0.32</mysql-connector-java.version>  
    <c3p0.version>0.9.5.2</c3p0.version>  
    <junit.version>4.12</junit.version>  
    <jackson.version>2.9.0</jackson.version>  
    <slf4j.version>1.6.6</slf4j.version>  
    <log4j.version>1.2.16</log4j.version>  
  </properties>  
  
  <dependencies>  
    <!--spring相关-->  
    <dependency>  
      <groupId>org.springframework</groupId>  
      <artifactId>spring-context</artifactId>  
      <version>${spring.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>com.alibaba</groupId>  
      <artifactId>druid</artifactId>  
      <version>1.2.23</version> <!-- 使用最新的稳定版本 -->  
    </dependency>  
  
    <dependency>  
      <groupId>org.aspectj</groupId>  
      <artifactId>aspectjweaver</artifactId>  
      <version>${aspectjweaver.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>org.springframework</groupId>  
      <artifactId>spring-jdbc</artifactId>  
      <version>${spring.version}</version>  
    </dependency>  
    <!--支持TypeScript-->
    <dependency>  
      <groupId>org.springframework</groupId>  
      <artifactId>spring-tx</artifactId>  
      <version>${spring.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>org.springframework</groupId>  
      <artifactId>spring-test</artifactId>  
      <version>${spring.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>org.springframework</groupId>  
      <artifactId>spring-webmvc</artifactId>  
      <version>${spring.version}</version>  
    </dependency>  
    <!--servlet-->  
    <dependency>  
      <groupId>javax.servlet</groupId>  
      <artifactId>servlet-api</artifactId>  
      <version>${servlet-api.version}</version>  
    </dependency>  
    <!--mybatis相关-->  
    <dependency>  
      <groupId>org.mybatis</groupId>  
      <artifactId>mybatis</artifactId>  
      <version>${mybatis.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>org.mybatis</groupId>  
      <artifactId>mybatis-spring</artifactId>  
      <version>${mybatis-spring.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>mysql</groupId>  
      <artifactId>mysql-connector-java</artifactId>  
      <version>${mysql-connector-java.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>com.mchange</groupId>  
      <artifactId>c3p0</artifactId>  
      <version>${c3p0.version}</version>  
    </dependency>  
    <!--测试相关-->  
    <dependency>  
      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>${junit.version}</version>  
      <scope>test</scope>  
    </dependency>  
    <!--jackson相关-->  
    <dependency>  
      <groupId>com.fasterxml.jackson.core</groupId>  
      <artifactId>jackson-core</artifactId>  
      <version>${jackson.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>com.fasterxml.jackson.core</groupId>  
      <artifactId>jackson-databind</artifactId>  
      <version>${jackson.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>com.fasterxml.jackson.core</groupId>  
      <artifactId>jackson-annotations</artifactId>  
      <version>${jackson.version}</version>  
    </dependency>  
    <!--log4j日志-->  
    <dependency>  
      <groupId>org.slf4j</groupId>  
      <artifactId>slf4j-api</artifactId>  
      <version>${slf4j.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>org.slf4j</groupId>  
      <artifactId>slf4j-log4j12</artifactId>  
      <version>${slf4j.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>log4j</groupId>  
      <artifactId>log4j</artifactId>  
      <version>${log4j.version}</version>  
    </dependency>  
    <dependency>  
      <groupId>org.junit.jupiter</groupId>  
      <artifactId>junit-jupiter</artifactId>  
      <version>5.8.1</version>  
      <scope>compile</scope>  
    </dependency>  
  </dependencies>  
  
  <build>  
    <finalName>jiajumail3</finalName>  
  </build>  
</project>
```

2本质上就是对前面学过的框架进行整合配置
* 配置Spring , resources/applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"  
       xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:aop="http://www.springframework.org/schema/aop"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd  
           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">  
    <!--扫描包-->  
    <context:component-scan base-package="ohmygod.furn"></context:component-scan>  
      
    <!--扫描mvc注解-->  
    <mvc:annotation-driven></mvc:annotation-driven>  
      
    <!--jdbc模板-->  
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">  
        <property name="dataSource" ref="dataSource"/>  
    </bean>  
      
    <!--导入外部资源-->  
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>  
      
    <!--注入数据源,要搭配jdbc配置文件,所以上面要导入外部资源-->  
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">  
        <property name="url" value="${jdbc.url}"></property>  
        <property name="username" value="${jdbc.username}"></property>  
        <property name="password" value="${jdbc.password}"></property>  
        <property name="driverClassName" value="${jdbc.driver}"></property>  
    </bean>  
  
    <!--mybatis的整合,需要引入mybatis-spring适配包-->  
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">  
  
        <!-- 方法1：保留全局配置文件指定Mybatis全局配置文件-->  
        <property name="configLocation" value="classpath:mybatis-config.xml"/>  
  
        <!-- 3.2 指定Mapper配置文件位置-->  
       <property name="mapperLocations" value="classpath:mappers/*Mapper.xml"/>  
  
        <!--3.3装配数据源(我们在上面配置的数据源)-->  
        <property name="dataSource" ref="dataSource"/>  
    </bean>  
  
    <!--配置扫描器,将mybatis接口的实现加入到ioc容器中-->  
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">  
        <property name="basePackage" value="ohmygod.furn.dao"/>  
    </bean>  
  
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">  
        <!--装配数据源（要能管事务就要控制住数据源，数据源中连接，回滚等操作）-->  
        <property name="dataSource" ref="dataSource"/>  
    </bean>  
  
    <!--开启基于注解的声明式事务-->  
    <tx:annotation-driven transaction-manager="transactionManager"/>  
    这只是其中的一种方式-->  
  
    <!--使用事务操作,这里采用(xml配置+切入表达式),指定切入点-->  
    <tx:advice id="txAdvice" transaction-manager="transactionManager">  
        <tx:attributes>  
           <!-- *代表所有的方法都是事务方法-->  
            <tx:method name="*"/>  
            <!--以get开始的所有方法,我们认为是只读,进行调优-->  
            <tx:method name="get*" read-only="true"/>  
        </tx:attributes>  
    </tx:advice>  
    <aop:config>  
        <!--切入表达式-->  
        <aop:pointcut id="txPoint" expression="execution(* ohmygod.furn.service..*(..))"/>  
        <!--配置事务增强/规则: 使用txAdvice指定规则对txPoint进行切入-->  
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPoint"/>  
    </aop:config>  
</beans>
```
大多数是在写mybatis的配置,配置jdbc,引入mybatis-spring整合配置,然后将mybatis接口的实现放入spring容器中,最后是事务之类的配置
**他的主要功能有
1.  配置自动扫描的包（service层）
2.  加载外部属性配置文件（jdbc.properties）  
    2.1 配置数据源
3.   配置SqlsessionFactoryBean(整合mybatis)  
    3.1指定mybatis全局配置文件  
    3.2 指定Mapper配置文件位置  
    3.3装配数据源（因为以往都是在mybatis中要配置的）
4.  配置扫描mapper接口
```properties
jdbc.driver=com.mysql.cj.jdbc.Driver  
jdbc.url=jdbc:mysql://127.0.0.1:3306/home_furnishing?serverTimezone=Asia/Shanghai&amp;useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8  
jdbc.username=root  
jdbc.password=zhouxinji.119
```
3.SpringMVC的整合配置
正常来说需要先配置web.xml文件,但是为了简显易懂这里先配置webapp/WEB-INF/springDispatcherServlet-servlet.xml
```xml
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:context="http://www.springframework.org/schema/context"  
       xmlns:mvc="http://www.springframework.org/schema/mvc"  
       xsi:schemaLocation="  
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">  
  
        <!--扫瞄哪些需要实例化的包-->  
        <context:component-scan base-package="ohmygod"  
            use-default-filters="false">  
            <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>  
        </context:component-scan>  
      
    <mvc:annotation-driven />  
      
    <!--配置视图解析器-->  
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">  
        <!--配置属性suffix 和 prefix-->        <property name="prefix" value="/WEB-INF/pages/"/>  
        <!--重点,这里是前后端分离的,不需要jsp,vue会解析成html然后返回给前端界面-->  
        <property name="suffix" value=".html"/>  
        <!--调整优先级-->  
    </bean>  
</beans>
```
主要还是在配置扫包和视图解析器
这个配置文件的主要功能有
1. 扫描（controller）组件
2. thymeleaf视图解析器
3. 视图控制器view-controller
4. 开放静态资源访问
5. 注解驱动

4.Mybatis的配置
resources/mybatis-config.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <typeAliases>  
        <package name="ohmygod.furn.dao"/>  
    </typeAliases>  
</configuration>
```

5.重点,配置web.xml处理三大框架的关系
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee  
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"         version="4.0">  
  
  <context-param>  
    <param-name>contextConfigLocation</param-name>  
    <param-value>classpath:applicationContext.xml</param-value><!--放在类路径上,自然而然要放在resource中-->  
  </context-param>  
  <!--ContextLoderListener是一种监听器,监听web容器的启动,自动装配ApplicationContexts的配置信息  
  它实现了ServetContextListener接口,在web.xml配置该监听,启动容器时,会默认执行它实现的方法-->  
  <listener>  
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>  
  </listener>  
    
  <!--配置前端控制器所有用户的请求都会通过它的处理-->  
  <servlet>  
      <servlet-name>springDispatcherServlet</servlet-name>  
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
      <load-on-startup>1</load-on-startup>  
  </servlet>  
  <servlet-mapping>  
    <servlet-name>springDispatcherServlet</servlet-name>  
    <url-pattern>/</url-pattern>  
  </servlet-mapping>  
    
  <filter>  
    <filter-name>CharacterEncodingFilter</filter-name>  
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>  
    <init-param>  
      <param-name>encoding</param-name>  
      <param-value>utf-8</param-value>  
    </init-param>  
  </filter>  
  <filter-mapping>  
    <filter-name>CharacterEncodingFilter</filter-name>  
    <url-pattern>/*</url-pattern>  
  </filter-mapping>  
  <filter>  
    <filter-name>hiddenHttpMethodFilter</filter-name>  
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>  
  </filter>  
  <filter-mapping>  
    <filter-name>hiddenHttpMethodFilter</filter-name>  
    <url-pattern>/*</url-pattern>  
  </filter-mapping>  
</web-app>
```
## 前端环境搭建
#### 搭建vue前端环境
#### 根据vue创建基础界面
## 具体的CRUD操作
#### 增
1.快速入门的增
```java
public class XxxMapper{
	
}
```

#### 查
#### 改
#### 删
