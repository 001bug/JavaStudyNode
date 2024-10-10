# Redis的简介
**redis的相关资料**
官网: https://redis.io/
中文地址: http://redis.cn/
下载地址: https://redis.io/download

**Redis的特性**
1.高并发 2.高可用 3.高性能 4.海量用户 解决关系型数据库(mysql)的弊端
* mysql数据库存在性能瓶颈: 磁盘IO性能低下
* 扩展瓶颈: 数据关系复杂, 扩张性差 , 不便于大规模集群
Redis的优势
* 内存存储-降低磁盘IO次数
* 不存储关系-仅存储数据-数据间关系(map关系)
总得来说: Redis是用c语言开发的一个开源的高性能键值对(key-value)数据库

**Redis的具体特征**
1.数据间没有必然的关联关系

2.高性能.

3.支持多种数据结构
* 字符串类型 string
* 列表类型 list
* 散列类型 hash
* 集合类型 set
* 有序集合类型 sorted_set

4.持久化支持 , 可以进行数据灾难恢复

5.应用场景非常广泛
1.为高频访问的数据加速查询, 如热点商品 , 热点新闻 , 热点咨询 , 推广等
2.任务队列 , 如秒杀 , 抢购 , 购票排队等
3.即使消息查询, 如排行榜 , 各类网站访问统计
4.时效性信息控制 , 如验证码控制 , 投票控制等
5.分布式数据共享，如分布式集群架构中的session 分离
6.消息队列
7.分布式锁
# NoSQL数据库
Not-Only SQL(泛指非关系型的数据库), 作为关系型数据库的补充

**作用**: 应对在海量用户和海量数据的情况下 , 带来的数据处理问题

**NoSQL数据库的特点**
可扩容，可伸缩
大数据量下高性能
灵活的数据模型
高可用 (高可用的意思是,在数据遇到损坏时,有很好的保护以及恢复机制)

**常见的NoSQL数据库**
Redis  memcache  HBase MongoDB
# Redis的下载和安装
下载地址 :https://redis.io/download
在实际工作中 , Redis都是在linux环境下工作

**安装具体步骤**
1.先让Linux网络环境为外网形式

2.下载安装最新版的gcc编译器
`yum install gcc` 前提可以尝试`ping www.baidu.com`测试一下网络情况
`gcc --version` 测试gcc版本

3.下载redis-6.2.6.tar.gz上传到/opt目录
![](assest/Pasted%20image%2020241010202454.png)

4.进入到/opt目录 , 执行解压命令: tar -zxvf redis-6.2.6.tar.gz
5.解压完成后 , 进入目录: `cd redis-6.2.6`
![](assest/{BC00EEAC-9C0C-4DA6-A9D1-A7AA37DBA04F}.png)
6.在redis-6.2.6目录下 , 执行make命令(编译指令)
7.执行: make install , 进行安装
![](assest/Pasted%20image%2020241010203442.png)
到此 , 安装目录在/usr/local/bin
8.查看安装目录的文件
![](assest/{99A5CE21-D231-467D-8C5C-A73DBDD9BD59}.png)
redis-benchmark:性能测试工具，可以在自己机器运行，看看自己机器性能如何
redis-check-aof：修复有问题的AOF 文件
redis-check-dump：修复有问题的dump.rdb 文件
redis-sentinel：Redis 集群使用
redis-server：Redis 服务器启动命令
redis-cli：客户端，操作入口

**Redis后台启动&使用**
1.拷贝一份redis.conf到其他目录 , 比如/etc目录 
`cp redis.conf /etc/redis.conf`这个要在opt/redis-6.2.6目录下执行
2.