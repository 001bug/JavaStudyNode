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