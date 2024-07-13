## 1逻辑架构

##### 逻辑架构剖析(运行流程)
MySQL处理一个请求的过程
![](Pasted%20image%2020240713203542.png)
==MySQL官方系统结构图==
![](Pasted%20image%2020240713203609.png)
1. 连接层
Connection Pool:线程池,通过tcp连接,建立交互
Management Services:基础服务组件,查询权限,类似于给请求附上token

2. 服务层
SQL Interface:SQL接口,处理SQL语句,还可以返回SQL语句的查询结果\
Parser: 解析器,对SQL语句进行语法分析,先检查词法,然后语法,返回语法树,==校验权限==
Optimizer: 查询优化器,确定执行计划,选择索引,  查询优化器中，可以分为逻辑查询优化阶段和物理查询优化阶段。
```
SELECT id,name FROM student WHERE gender = '女';
这个SELECT查询先根据WHERE语句进行选取，而不是将表全部查询出来以后再进行gender过
滤。 这个SELECT查询先根据id和name进行属性投影，而不是将属性全部取出以后再进行过
滤，将这两个查询条件连接起来生成最终查询结果。
```
Caches & Buffers： ==查询缓存==组件. MySQl8.0已经抛弃.比如缓存一条SELECT语句的执行结
果，如果能够在其中找到对应的查询结果，那么就不必再进行查询解析、优化和执行的整个过
程了，直接将结果反馈给客户端。

3. ==存储引擎层==
Storage Engines:真正的负责了MySQL中数据的存储和提取，对物理服务器级别维护的底层数据执行操作

4. 存储层:所有的数据，数据库、表的定义，表的每一行的内容，索引存在文件系统中

##### 实用查看profile
```
查看profile mysql>select @@profiling;
开启profile mysql>set profiling=1;
查看profile mysql> show profiles;//不详细
    profile mysql> show profile;//详细
指定查询 mysql> show profile for query id
```
![](Pasted%20image%2020240713210332.png) 不详细
![](Pasted%20image%2020240713210348.png) 详细

##### 数据库缓冲池
数据库缓冲池（Database Buffer Pool）是数据库管理系统（DBMS）中用于==临时存储数据的内存区域==。它的主要目的是提高数据库系统的性能，通过减少直接从磁盘读取和写入数据的次数来加速数据访问。
![](Pasted%20image%2020240713211027.png)
MyISAM存储引擎在缓冲池只是存储索引     InnoDB缓冲池是存储具体数据的

## 2存储引擎

##### 实用技巧
查看存储引擎,设定默认存储引擎

##### 存储引擎介绍
1. InnoDB 引擎:
特点:MySQL5.5往后的默认引擎,具有事务功能,除非有非常特别的原因需要使用其他的存储引擎，否则应该优先考虑InnoDB引擎。
对比MyISAM的存储引擎， InnoDB写的处理效率差一些，因为InnoDB不仅缓存索引还要缓存真实数据 , 所以会占用更多的磁盘空间用来保存数据和索引。文件目录有.ibd文件,存储索引和数据

2. MyISAM 引擎
MyISAM有全文索引、压缩、空间函数(GIS)等，但==MyISAM 不支持事务==、行级锁、外键，有一个毫无疑问的缺陷就是崩溃后无法安全恢复,
count() 的查询效率很高 , 适用只读应用或者以读为主的业务,文件目录表.frm表结构,MYD存数据,MYI存索引

3. Archive 引擎
Archive有备份/时间点恢复（在服务器中实现，而不是在存储引擎中）压缩数据,行锁 , 使用于数据存档

4. Blackhole 引擎：丢弃写操作，读操作会返回空内容
5. CSV 引擎：存储数据时，以逗号分隔各个数据项

**MyISAM和InnoDB对比**
![](Pasted%20image%2020240713215417.png)

## 3索引的数据结构
