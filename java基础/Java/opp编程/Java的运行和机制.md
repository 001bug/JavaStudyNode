**[[jvm]]:1,是java的核心机制,全名(JVM java virtual machine) , JVM是一个虚拟计算机,具有指令集并使用不同的存储区域. 负责执行指令,管理数据,内存,寄存器,**

**2,对于不同的平台,有不同的[虚拟机].**

**3java虚拟机机制屏蔽了底层运行平台的差别**

**2,[[JDK]]**

**JDK(**java Development Kit java开发工具包**)**

**JDK**是提供给java开发人员使用的,其中包含了java的开发工具,也包含了JRE.

**3.[[JRE]]**

1,JRE(java runtime Environment java运行环境)

2.分为Server JRE(服务器jre) SE(用于桌面和简单服务器应用的java平台) EE(复杂服务器应用的java平台)

3,包括java虚拟机(JVM java virtual Machine)和java程序所需的核心类库等,JRE=JVM+java核心类库(类)

**JDK = JRE + 开发工具集(javac, java编译工具)**

**JRE = JVM +java SE 标准类库**

**JDK = JVM + java SE标准类库+开发工具集**

[[开发工具集]]:javac(java Compiler):用于将java源代码编译成java字节码文件(java.class)

java(Java Interpreter):用于执行 Java 字节码文件。它是 Java 程序的运行时环境，负责解释和执 行字节码。

[javadoc](java Documentation Generatror): 用于生成 Java 代码的文档。开发者可以使用特定 的注释格式，然后运行 javadoc 工具生成 HTML 格式的文档。

[jar](java Archive): 用于创建和管理 Java 归档文件（.jar 文件），这些文件通常包含了多个 Java 类文件和其他资源。

[jdb](java Debugger): java调试器,用于调试java程序,支持设置断点,单步执行等调试操作

[javap](java Class File Disassembler): 用于反汇编java字节码文件,显示类的结构和内容

[jps](Java Process Status): 显示当前系统中所有java 进程的状态信息

[jstack](Java Stack Trace): 生成Java进程的线程堆跟踪信息,用于分析和调试多线程应用程序。

此电脑图标的出现方法:右键-->个性化--->桌面图标

查看操作系统 对此电脑点右键.看看是几位操作系统

**对jdk文件的解读,**

src.zip是[java源代码]

**配置环境变量(一般用于dos系统,简称控制台,win+r)**

当前执行的程序在当前目录下如果不存在,win10系统会在系统中已有的一个名位Path的环境变量

1.我的电脑--->属性--->高级系统设置--->环境变量

2.增加Java_HOME环境变量(代表指向JDK的安装目录)

3.编辑path环境变量,增加%JAVA_HOME%\bin

administrator的用户变量只是对于当前用户起作用,而系统变量对于所有用户都起作用