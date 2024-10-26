# Nginx的引入以及介绍
**Nginx的介绍**
Nginx是一个高性能的HTTP和反向代理web服务器 , 它具有高性能 , 处理高并发 , 支持热加载(不停服务器的情况下加载配置文件)详细介绍:https://lnmp.org/nginx.html
官方文档:https://nginx.org/en/docs/

**实际需求**
1.访问不同的微服务
![](assest/Pasted%20image%2020241026082853.png)
访问同一个产品但是 , 不同的服务 , 并且服务分布在不同的服务器中
2.轮询访问服务
![](assest/Pasted%20image%2020241026083010.png)面对这种需求可以使用Nginx的反向代理 , 负载均衡 , 动静分离
![](assest/Pasted%20image%2020241026083116.png)
# Nginx核心功能
