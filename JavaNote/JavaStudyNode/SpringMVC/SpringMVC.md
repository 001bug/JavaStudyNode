### SpringMVC的快速入门
1.引入包
![](assest/Pasted%20image%2020240724101607.png)
2.配置web.xml文件(主要是配置前端控制器)
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee  
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"         version="4.0">
    <servlet>  
    <servlet-name>springDispatcherServlet</servlet-name>  
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
    <load-on-startup>1</load-on-startup>  
	</servlet>
	
	<servlet-mapping>  
	    <servlet-name>springDispatcherServlet</servlet-name>  
	    <!--老师说明  
	    1. 这里我们配置的url-pattern是 / ,表示用户的请求都经过 DispatcherServlet  
	    2. 这样配置也这次rest 风格的url请求  
	    -->  
	    <url-pattern>/</url-pattern>  
	</servlet-mapping>
</web-app>
```
在配置前端控制器,所有的请求都要经过这个底层实例,要进行map映射处理
3.然后创建并配置springDispatcherServlet-servlet.xml文件(spring的容器文件),配置视图解析器,前端控制器
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:context="http://www.springframework.org/schema/context"  
       xmlns:mvc="http://www.springframework.org/schema/mvc"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
        http://www.springframework.org/schema/beans/spring-beans.xsd        http://www.springframework.org/schema/context        https://www.springframework.org/schema/context/spring-context.xsd        http://www.springframework.org/schema/mvc        https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--配置自动扫描包-->  
	<context:component-scan base-package="指定要扫描的包"/>

	<!--视图解析器的配置-->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">  
	    <!--配置属性suffix 和 prefix-->  
	    <property name="prefix" value="/WEB-INF/pages/"/>  
	    <property name="suffix" value=".jsp"/>  
	    <!--调整优先级-->  
	    <property name="order" value="10"/>  
	</bean>
</beans>
```
注意要放在web路径的根路径下
4.写servlet层
```java
@Controller//先把这个注入容器内管理
public class UserServlet{
	//编写方法,响应用户的请求
	@RequestMapping(value="/login")
	public String login(){
		/////
		return "页面";//会返回到视图解析器
	}
}
```
非常要注意,到底是在子目录还是在根目录
### REST请求风格
#### REST请求风格的概念
**简单概念**
是一种基于资源的架构风格
**REST的基本原则**
1.资源和URI唯一性
资源表示可以访问和操作的数据对象（例如用户、订单、产品等），每个资源由一个唯一的 URI
2.HTTP方法约定性
REST 使用标准的 HTTP 方法来对资源进行操作：
- **GET**：用于检索资源。
- **POST**：用于创建资源。
- **PUT**：用于更新资源。
- **DELETE**：用于删除资源。
- **PATCH**：用于部分更新资源。
#### REST请求风格在SpringMVC的实现
1.首先要引入REST的核心过滤器(HiddenHttpMethodFilter),这个过滤器要web.xml中配置,它能够把前端发送过来的put请求变为springmvc,rest格式的请求.2.注意只能对post请求方式进行转换
```xml
<filter>  
    <filter-name>hiddenHttpMethodFilter</filter-name>  
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>  
</filter>  
<filter-mapping>  
    <filter-name>hiddenHttpMethodFilter</filter-name>  
    <url-pattern>/*</url-pattern>  
</filter-mapping>
```
2.然后再springDispatcherServle-servlet.xml中配置,映射动态请求**JSR303校验**能够对javabean的进行规范化的验证,在Spring中是自动校验的
```xml springDispatcherServlet-servlet.xml
<!--支持SpringMVC的高级功能，比如JSR303校验, 映射动态请求-->  
<mvc:annotation-driven></mvc:annotation-driven>  
<!--将springmvc不能处理的请求，交给tomcat处理,比如css, js-->  
<mvc:default-servlet-handler/>
```
3.写Servlet层,Handler
```java
@RequestMapping("/user")
@Controller
public class XxxHandler{
	//查询[GET]  
	@RequestMapping(value = "/book/{id}", method = RequestMethod.GET)  
	public String getBook(@PathVariable("id") String id) {  
	    System.out.println("查询书籍 id=" + id);  
	    return "success";  
	}
	//添加[POST]  
	@PostMapping(value = "/book")  
	public String addBook(String bookName) {  
	    System.out.println("添加书籍 bookName== " + bookName);  
	    return "success";  
	}
	//删除[DELETE]  
	@RequestMapping(value = "/book/{id}", method = RequestMethod.DELETE)  
	public String delBook(@PathVariable("id") String id) {  
	    System.out.println("删除书籍 id= " + id);  
	    //return "success"; //[如果这样返回会报错 JSPs only permit GET POST or HEAD]  
	    //1. redirect:/user/success重定向  
	    //2. 会被解析成 /springmvc/user/success  
	    return "redirect:/user/success";  
	}
	//修改[PUT]  
	@PutMapping(value = "/book/{id}")  
	public String updateBook(@PathVariable("id") String id) {  
	    System.out.println("修改书籍 id=" + id);  
	    return "redirect:/user/success";  
	}
}
```
难度就是删除操作,注意,
### SpringMVC文件上传
1.配置文件上传解析器,先配置xxxservlet.xml文件
```xml
<bean
	id="multipartBesolver"
	class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
</bean>
```
2.配置中文过滤器
在xxxservlet.xml文件中配置,要放在其它Servlet前
![](assest/Pasted%20image%2020240714182244.png)
3.样例模版
```java
@RequestMapping(value = "/url")
public String fileUpload(@RequestParam(value = "file") MultipartFile file, HttpServletRequest request, String introduce) throws IOException{
	String originalFilename = file.getOriginalFilename();
	String fileFullPath =  
        request.getServletContext().getRealPath("/要存到哪/" + originalFilename);
    File saveToFile = new File(fileFullPath);
    file.transferTo(saveToFile);
    return "ok";
}
```

对样例模版的解释'
### 自定义拦截器
##### 自定义拦截器实例
拦截器:对请求会拦截
过滤器由tomcat服务器接管,拦截器由spring接管
1.写样例模版
```java
@Component
public class 类名 implements HandlerInterceptor{
	@Override
	public boolean preHandle(HttpServletRequest arg0,   HttpServletResponse arg1,Object arg2) throws Exception{
	if(){
		return false;
	}
	else{
		return true;
	}
}
	@Override
	public void postHandle(HttpServletRequest arg0,
HttpServletResponse arg1,Object arg2, ModelAndView arg3){
	//
}
	@Override
	public void afterCompletion(HttpServletRequest arg0,
HttpServletResponse arg1, Object arg2, Exception arg3) throws Exception{
	//
	}
}
```
2.在xxxservlet.xml中配置(这里的类名和上面的类名是一样的)
第一种,所有拦截
```xml
<mvc:interceptors>
	<ref bean="首字母小写类名"/>//所有方法都拦截
</mvc:interceptors>
```
第二种配置方法,指定拦截
```xml
<mvc:interceptors>
	<mvc:interceptor>
	   <mvc:mapping path="/被拦截的请求"/>
	    <ref bean="首字母小写类名"/>
	</mvc:interceptor>
</mvc:interceptors>
```
第三种,[范围拦截](web路径)
```xml
<mvc:interceptors>
	<mvc:interceptor>  
	    <mvc:mapping path="/h*"/>  
	    <mvc:exclude-mapping path="/hello"/> //不拦截的url
	    <ref bean="首字母小写类名"/>  
	</mvc:interceptor>
</mvc:interceptors>
```
##### 对样例模版的解读
执行流程
![](assest/Pasted%20image%2020240714190901.png)
自定义拦截器执行流程说明
1. 如果preHandle 方法返回false, 则不再执行目标方法, 可以在此指定返回页面
2. postHandle 在目标方法被执行后执行. 可以在方法中访问到**目标方法返回**的
ModelAndView 对象
3. 若preHandle 返回true, 则afterCompletion 方法在**渲染视图**之后被执行.
4. 若preHandle 返回false, 则afterCompletion 方法不会被调用
5. 在配置拦截器时，可以指定该拦截器对哪些请求生效，哪些请求不生效

**首字母小写是因为在bean容器内部,如果你不指定组件的名字默认是小写的**
##### 多个拦截器的流程分析
![](assest/Pasted%20image%2020240714202359.png)
在xxx-servlet.xml中按顺序配置 .  注意,BInterceptor中的preHandle是flase直接进入AInterceptor的after方法,目标方法都不执行
### 异常处理
**1.局部异常**
样例模版
```java
@ExceptionHandler({异常类,异常类..})
public 结合实际 方法名(Exception ex,HttpServletRequest request){
	//业务逻辑
}
```
样例模版的解释
这个方法放在一个类中,所以是局部异常
==执行流程==,遇到异常,然后通过ExceptionHandlermethodResolver在本类中查找是否有ExceptionHandler注释的方法,然后看看里面的异常类是否符合,最后被Exception ex获取
==Debug流程==,ctrl+n搜ExceptionHandlermethodResolver---->找resolveMethodByException,然后下断点,然后通过get..方法拿到异常, 接着返回局部异常下断点放行

**2.全局异常**
样例模版
```java
@ControllerAdvice
public class GlobalException{
	@ExceptionHandler({异常类,异常类..})
    public ?localException(Exception ex,HttpServletRequest request){
		//业务
		return ?;
	}
}
```
样例模版的解释
==执行流程==这个全局异常要在局部异常没有生效的情况下才会调用.如果全局异常还不能捕获,那就用tomcat自带的异常处理
==Debug流程==,先在找到ExceptionHandlermethodResolver,然后再本类中查找有ExceptionHandler注释的方法.如果有,那就不执行全局异常,如果没有就执行全局异常.返回的时候是Null,然后调用resolveMehodByThrowable去找全局异常,最后追到for(Map.Entry< ControllerAdviceBean,ExceptionHandlerMethodResolver >)这里遍历找全局异常处理

**3.自定义异常**
样例模版
```java
@ResponseStatus(reason="异常信息",value=HttpStatus.状态信息)
public class xxException extends 异常类{
	public xxException(){
	}
	public xxException(String message){
		super(message);
	}
}
```
样例模版的解释
==执行流程==:跟普通的异常一样,注意:如果想要在全局异常或者局部异常使用,那就需要在自定义异常增加构造方法,因为注释里面的报错信息是给tomcat使用的
==Debug流程==:也是先找到ExceptionHandlermethodResolver中的resolveMethodByExceptionType(查method,查return)  , 然后调用resolveMehodByThrowable去找全局异常,到for , 在for中查找advice , 然后可放行
如果没有放在全局异常处理,是不会走到样例模版中构造方法的

**4.统一处理异常**
样例模版
1.配置xxx-servlet.xml
![](assest/Pasted%20image%2020240715105104.png)
==注意==:arrEx是和视图解析器的配置是一起作用的,那么arrEx的url受到视图解析器配置的影响

样例模版解析:统一处理异常只有在全局异常和局部异常都不发生的作用下起作用
< prop key >这个标签中的java.lang.ArrayIndexOutOfBoundsException 表示是可以接受的异常处理,在这个模版中只有数组越界异常才能返回自定义界面.  如果想要处理所有异常,改成java.lang.Exception

**异常的处理优先级** 局部>全局>统一>tomcat
### 源码执行流程分析
