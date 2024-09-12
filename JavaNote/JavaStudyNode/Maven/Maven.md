## Maven的引入以及初步使用
**Maven解决的问题**
1.非常方便的引入相关开发包
2.可以把项目打包成可以发布的[jar]([jar])
3.解决版本冲突
4.兼容性问题 , 比如Spring5.x需要使用jackson-2.9.x.jar的包处理json
5.项目很大 , 包括多个模块 , 多个模块有共有的jar
**创建maven项目**
1.用idea正常的创建maven项目
2.设置自己的项目坐标
## Maven的下载和安装
**Maven的文件目录**
![](assest/Pasted%20image%2020240903163946.png)
如果需要使用指令的形式使用maven的功能
1.进入maven文件中bin目录 , 然后打开命令窗口
2.配置环境变量

注意:Maven的使用需要有jdk作为依赖 , 因为maven本身是用java语言写的. Maven使用常用功能打包时 , Maven会用到java程序
## Maven的核心概念
### pom.xml文件
#### pom.xml文件的解读
1`modelversion`解读
```xml
<modelVersion>4.0.0</modelVersion>
```
这是pom的模型版本 , 通常是4.0.0 , 不常更改

2`<groupId>`
```xml
<groupId>com.example</groupId>
```
项目的组织或公司的标识符，通常是一个反向域名结构，用来唯一标识项目归属。与 `artifactId` 共同唯一标识一个项目。

3.`<artifactId>`
```xml
<artifactId>my-app</artifactId>
```
项目的名称，表示构建出的 JAR 或 WAR 文件名。和 `groupId` 结合后形成项目唯一标识符。

4.`<version>`
```xml
<version>1.0.0</version>
```
项目的版本号，用于管理和区分不同版本的构建。一般使用 `major.minor.patch` 语义化版本。

5.`<scope>`
`<scope>`标签用于定义依赖项的作用范围 . 通俗讲 , 定义它在哪些阶段可用或可见. 不同的scope决定了依赖在编译 , 测试 , 运行等不同阶段的可用性.
1. compile(默认值) . 默认作用域 , 作用在编译 , 测试 , 运行时都可用.
2. provided. 依赖在编译和测试时可用 , 在运行时必须由运行环境(如服务器) 提供
3. `runtime`依赖在运行时可用 , 但在编译时不可用. 例如数据库驱动
4. test 在测试阶段可用 , 编译和运行时不可见
5. system 类似`provided` , 但依赖的jar文件必须显示指定一个路径 , 且不会通过仓库下载. 使用本地特定位置的jar文件时 , 尽量避免使用此scope

6`<packaging>`
```xml
<packaging>jar</packaging>
```
定义项目打包方式。常见的值有 `jar`、`war`、`pom`。如果不定义，默认是 `jar`。

7.`dependencies`
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.6.4</version>
    </dependency>
</dependencies>
```
项目所依赖的第三方库

8.`<dependencyManagement>`
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>common-dependency</artifactId>
            <version>1.0.0</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```
用于声明项目的依赖版本 , 子项目可以继承这些版本定义而不必再每个子项目中重新定义依赖的版本号

9.`<repositories>`
定义项目所使用的Maven仓库地址 , 通常不需要特别配置 , maven默认使用中央仓库

10.`<properties>`
```xml
<properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```
定义项目中的一些全局属性 , 可以在其他标签中使用 . 通常用于配置Maven编译插件的java版本信息
### Maven的生命周期
### 依赖
## Maven的常用设置
### 设置Maven本地仓库缓存
**查找或者更改Maven从中央仓库下载到本地的存储路径**
file-->setting-->maven
![](assest/Pasted%20image%2020240903150039.png)
**Maven home path** 单选框用于指定 Maven 的安装路径。这个路径告诉 IDE 使用哪个 Maven 版本和配置来构建和管理项目
* 手动去指定 , 那就使用指定的maven 
* 不去指定 , 那就使用idea内置的maven
**Local repository**就是远程中央仓库下载到本地的位置
**User settings file** 用于指定Maven的用户配置文件路径 , 通常是`settings.xml`文件 . 这个文件定义了Maven的全局配置 , 例如仓库镜像 , 代理设置 , 插件设置
## Maven的常用功能
### Maven打包
**使用Maven对项目进行打包**
![](assest/Pasted%20image%2020240903152144.png)
双击package . 想要打包成哪种形式 , 在pom.xml文件中配置`<packaging>`
应用场景
1.应用程序的发布和分发
将项目打包成一个可执行的`.jar`文件, 比如 . 工具类库
2.部署到服务器
对于需要部署到服务器的项目(如web应用) , 打包后的`.war` , `.jar`文件可以直接上传到应用服务器中进行部署.
比如:
* Web应用 , 通过Maven打包成`.war`文件 . 然后发布到tomcat , jetty等服务器
* 微服务架构 , 将每个微服务模块打包为独立的可运行 `.jar`，部署到云端或服务器上运行。