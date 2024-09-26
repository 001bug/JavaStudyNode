### WEB开发通信协议-HTTP协议
#### HTTP基础概念
HTTP协议是客户端和服务器之间用于传输数据的协议, 他是一种无状态协议, 每次请求都是独立的, 服务器不记得上一次的请求状态. 主要涉及到HTTP请求分析,以及响应分析. http是TCP/IP协议的一个应用层协议, http也是我们web开发的基础
#### HTTP请求与响应的分析
##### 请求报文
包括请求行 , 请求头 , 请求体
* 请求行: 包括请求方法(GET, POST等), URL , HTTP版本
* 请求头: 传递客户端信息(User-Agent), 授权信息, 内容类型等
* 请求体: 在POST, PUT等方法下用于传递数据
![](assest/Pasted%20image%2020240925193658.png)
post方式的请求报文
![](assest/Pasted%20image%2020240925194910.png)
注意: 在请求行上并没有直接携带请求信息
HTTP请求体式客户端发送给服务器的实际数据内容 , 通常出现在POST, PUT, PATCH等请求方法中. 请求体可以包含多种类型的数据, 比如JSON, XML, 表单数据, 二进制文件
我们可以先看`Content-Type`看看请求体的类型是什么, 然后下几行就可能有请求体
比如下面代码`{}`里面的内容
```http
POST /api/users HTTP/1.1
Host: example.com
Content-Type: application/json
Content-Length: 57
{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "age": 30
}
```

**GET请求分别有哪些?**
* form标签`method=get`指定
* a标签
* link标签引入css`[以get方式获取资源]`
* Script标签引入js文件`[以get方式来获取资源]`
* img标签引入图片`[以get请求获取图片]`
* iframe引入html页面
**POST请求有哪些**
* form标签method=post
**状态码**
![](assest/Pasted%20image%2020240925201730.png)
##### 响应报文
包括状态行, 响应头, 响应体
* 状态行: 包括HTTP版本, 状态码, 状态消息
* 响应头: 传递服务信息(Server), 内容类型 , 缓存控制
* 响应体: 服务器返回的实际内容, 如HTML, JSON等
![](assest/Pasted%20image%2020240925202205.png)
### URL路径中的变量和请求参数
**1.URL路径中的变量**
URL路径中的变量是URL的一部分(请求的一部分), 用于在请求的路径中嵌入动态值.
示例: 
* URL:`/users/123`
* 代码
```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {

    // {id} 表示 URL 中的动态部分
    @GetMapping("/{id}")
    public String getUserById(@PathVariable("id") String userId) {
        return "User ID: " + userId;
    }
}
```
* 结果: 返回`"USer ID : 123"`
### Tomcat
##### Tomcat的基本介绍和服务器的知识
1.概述:tomcat是一种web服务器，提供对jsp 和Servlet 的支持。它
是一种轻量级的javaWeb 容器（服务器),本质上是个java程序
2.Tomcat的目录结构
![](assest/Pasted%20image%2020240722185133.png)
1. server.xml 用于配置tomcat 的基本设置(启动端口，关闭端口, 主机名)
2. wex.xml 用于指定tomcat 运行时配置(比如servlet 等..)
3. webapps 目录是存放web 应用，就是网站,界面之类的
3.Tomcat服务器中部署web应用
* web应用的概念:
1. WEB应用是多个web资源的集合。简单的说，可以把web应用理解为硬盘上的一个目录，这个目录用于管理多个web资源。
2. Web应用通常也称之为web应用程序，或web工程，通俗的说就是网站。
3. 开发人员在开发web 应用时，按照规定目录结构存放这些文件。否则，在把web 应用交给**web 服务器管理**时，不仅可能会使web 应用无法访问，还会导致web 服务器启动报错。
4. 部署方式,直接将项目copy到web目录下. 2,通过配置文件部署 3,通过idea部署

4.浏览器访问web服务的过程
![](assest/Pasted%20image%2020240722190706.png)
