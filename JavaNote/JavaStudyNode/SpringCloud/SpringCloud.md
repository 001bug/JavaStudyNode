# SpringCloud的基本介绍
## 基本定义
官方文档: https://spring.io/projects/spring-cloud
SpringCloud是一个基于SpringBoot的微服务架构工具集, 帮助开发者构建和管理分布式系统 , 提供了一整套组件和工具, 简化微服务架构中的常见问题 , 如服务发现 , 配置管理 , 负载均衡 , 熔断 , 分布式追踪等. Spring Cloud 可以定义为一个**微服务架构解决方案**
示意图
![](assest/Pasted%20image%2020241025210303.png)
**SpringCloud的全面说明**
1.SpringCloud是源于Spring的 , 是更高层次和架构视角的大型项目, 宗旨在于构建一套标准化的微服务解决方案, 让架构师在使用微服务理念构建系统时, 面对各种环节的问题可以找到相对应的组件来处理
2.Spring Cloud 是Spring 社区为微服务架构提供的一个"全家桶" 套餐。套餐中各个组件之间的配合， 可以减少在组件的选型和整合上花费的精力，可以快速构建起基础的微服务架构系统，是微服务架构的最佳落地方案
3.高效解决与**分布式系统**相关的复杂性问题-网络问题 , 延迟开销 , 宽带问题 , 安全问题
4.拥有[[服务发现]]的能力-服务发现系统允许集群中的进程和服务找到彼此并进行通信
5.解决冗余问题-冗余问题经常发生在分布式系统中
6.解决负载均衡- 改进跨多个计算资源(例如计算集群 , 网络链接 , 中央处理单元)的工作负载分布
## 系统架构演变过程
.在没有SpringCloud时 , 程序员对大型项目进行模块划分 , 然后针对各个模块进行实现, 模块之间通过API调用, 耦合度高 , 不利于扩展和维护.
.后来将这种解决类似问题的方案统一了 . 分解成不同的服务比如(搜索服务/网关服务/配置服务/存储服务/发现服务等等). 各个服务通过分布式方式进行工作, 高效快速.稳定的完成复杂任务.
1.单机架构
![](assest/Pasted%20image%2020241026081443.png)
2.[[动静态分离]]架构: 静态缓存+文件存储
![](assest/Pasted%20image%2020241026081519.png)
3.分布式架构: 业务拆分+负载均衡
![](assest/Pasted%20image%2020241026081544.png)
4.微服务架构: 使用SpringCloud
![](assest/Pasted%20image%2020241026085722.png)
解读
* "微服务"来源于Martion Fowler的一篇名为Microservices的博客 . 微服务是系统架构中的一种设计风格. 它的宗旨是将一个原本独立的系统拆分成多个小型服务,这些小型服务都在独立的进程中运行, 服务之间通过基于HTTP的RESTfulAPI进行通信
* 被拆分成的每一个小型服务都围绕着系统中的某一些*耦合度*较高的业务功能进行构建, 并且每个服务都维护着自身的数据存储、业务开发、自动化测试案例以及独立部署机制。因为有轻量级的通信协作基础， 所以==这些微服务可以使用不同的语言来编写==, 这里我们使用java
## SpringCloud的核心组件图
![](assest/Pasted%20image%2020241026100100.png)
springcloud Alibaba文档地址:https://github.com/alibaba/spring-cloud-alibaba
spring cloud Netflix 文档地址:https://github.com/Netflix
![](assest/Pasted%20image%2020241026100317.png)
Nacos借用了很多Eureka的设计理念
## SpringCloud分布式示意图
**文档**: https://spring.io/microservices
示意图
![](assest/Pasted%20image%2020241026105759.png)
这是一个非常简洁的分布式微服务架构示意图. 但包含了大多主要的功能.
1.SpringCloud是微服务的落地
2.SpringCloud体现了微服务的弹性设计
3.微服务的工作方式一般是基于分布式的. SpringCloud仍然是Spring家族一员 , 可以解决微服务的分布式工作带来的各种问题
4.SpringCloud提供很多组件 , 如服务发现 , 负载均衡 , 链路中断 , 分布式追踪和监控. API gateway功能

**SpringCloud和SpringBoot的版本对应关系**
地址: https://spring.io/projects/spring-cloud
![](assest/Pasted%20image%2020241026111128.png)
## Spring Cloud组件选型
![](assest/Pasted%20image%2020241026111931.png)带`√`表示目前流行的 , 且功能强大的 , 时常更新的
带`×`表示目前不流行的 , 更新缓慢或者停止更新的
# Spring Cloud Alibaba基本介绍
**官方文档以及基本介绍**
官方文档; https://github.com/alibaba/spring-cloud-alibaba
中文文档: https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md

![](assest/Pasted%20image%2020241026113521.png)
**主要功能**
![](assest/Pasted%20image%2020241026113603.png)
**SpringCloud Alibaba核心组件**
![](assest/Pasted%20image%2020241026113922.png)
**选择SpringCloud Alibaba组件的原因**
1.SpringCloud部分组件停止维护和更新 , 给开发带来不便
2.SpringCloud部分环境搭建复杂 , 没有可视化界面 , 需要大量的二次开发和定制
3.SpringCloud配置复杂.

所以: 这里以Spring Cloud Alibaba组件为主 , Spring Cloud为辅.
# 微服务基础环境搭建
## 创建父工程, 聚合子微服务模块
创建一个父项目 , 然父项目区管理多个微服务模块
![](assest/Pasted%20image%2020241026181842.png)
1.创建父项目 , --选择灵活配置方式
![](assest/Pasted%20image%2020241026181931.png)
![](assest/Pasted%20image%2020241026181942.png)
2.项目的编码设置
![](assest/Pasted%20image%2020241026182055.png)
3.设置编译器
![](assest/Pasted%20image%2020241026182115.png)
这个`Target bytecode version` , 选择的版本会影响编译器生成的字节码版本 . 例如选择java8 , 编译器会生成与java8兼容的字节码. 确保这些`.class`文件可以在java8或者更高版本的JVM上运行.
一般用来匹配项目的需求 , 性能优化 , 向后兼容
4.然后删除`src`保留一个纯净的环境
父项目一般只用来管理子模块 , 并不需要编写源码 , 所以把src文件删掉是最为合适的

5.然后编写对应的pom.xml文件
这里使用到了maven的聚合工程的功能
`<packaging>pom</packaging>`: 表名这是一个父工程 .
`<dependencyManagement>` :
* 一种管理依赖版本号的方式 , 通常在项目`packaging`为POM的xml中使用.
* 使用`pom.xml`中的`dependencyManagement`元素能让所有的子项目中引用一个依赖 , Maven会沿着父子层次向上走 , 直到找到拥有`dependencyManagement`元素的项目, 然后会使用该配置文件指定的版本号
* 好处: 如果有多个子项目都引用同一样依赖 , 则避免了子项目还要声明版本号 , 当升级或者切换到另一个版本时 , 只需要在带有`<packaging>pom</packaging>`的容器里更新.
* 这个和`<dependency>`不一样 , 它不会直接引入依赖. 只负责管理依赖版本
* 只有子项目声明了对应的依赖并且没有指定具体版本 , 才会从父项目中继承下来.![](assest/Pasted%20image%2020241026205808.png) 这个体现在pom.xml文件中`<scope>`标签中
## 会员中心微服务模块
模块名: `member-server-provider-10000`
名字的含义: member-server表示人员服务 . `provider`表示生产者 , `10000`表示端口

**具体操作步骤**
1.创建子模块
![](assest/Pasted%20image%2020241027081238.png)
注意看父工程是否正确

2.查看父工程的pom.xml文件,看他掌管的子模块是否是刚刚创建的
![](assest/Pasted%20image%2020241027081431.png)
3.编写子模块的pom.xml文件
* `<parent>`标签 , 指明该子模块对应的父工程
* `<groupId>` 在子工程继承父工程的前提下,`groupId`不用写
* 发现,子模块springboot元素并没有指定版本 , 而且父模块也没有指定版本. 这个原因是 , 父模块指定了`spring-boot-dependencies`的版本 , 这个元素里面又指定了boot的版本
* [[`spring-boot-starter-actuator`]]: springboot程序的监控系统 , 可以实现系统的健康检测
* `spring cloud sleuth`:分布式系统中,链路追踪工具
