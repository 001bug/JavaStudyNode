### SpringMVC文件上传
1.配置文件上传解析器,先配置xxxservlet.xml文件
```
<bean
	id="multipartBesolver"
	class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
</bean>
```
2.配置中文过滤器
在xxxservlet.xml文件中配置,要放在其它Servlet前
![](assest/Pasted%20image%2020240714182244.png)
3.样例模版
```
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
对样例模版的解释\

### 自定义拦截器
##### 自定义拦截器实例
拦截器:对请求会拦截
过滤器由tomcat服务器接管,拦截器由spring接管
1.写样例模版
```
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
```
<mvc:interceptors>
	<ref bean="首字母小写类名"/>//所有方法都拦截
</mvc:interceptors>
```
第二种配置方法,指定拦截
```
<mvc:interceptors>
	<mvc:interceptor>
	   <mvc:mapping path="/被拦截的请求"/>
	    <ref bean="首字母小写类名"/>
	</mvc:interceptor>
</mvc:interceptors>
```
第三种,[范围拦截](web路径)
```
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
```
@ExceptionHandler({异常类,异常类..})
public 结合实际 方法名(Exception ex,HttpServletRequest request){
	//业务逻辑
}
```
样例模版的解释
这个方法放在一个类中,所以是局部异常
==执行流程==,遇到异常,然后通过ExceptionHandlermethodResolver在本类中查找是否有ExceptionHandler注释的方法,然后看看里面的异常类是否符合,最后被Exception ex获取
==Debug流程==,ctrl+n搜ExceptionHandlermethodResolver---->找resolveMethodByException,然后下断点,然后通过get..方法拿到异常, 接着返回局部异常下断点放行
