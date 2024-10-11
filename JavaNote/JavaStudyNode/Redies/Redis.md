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
2.修改`/etc/redis.con`后台启动设置`daemonize no 改成 yes`,并保存退出(推荐使用vim打开,使用vim的搜索工具)
![](assest/Pasted%20image%2020241010205653.png)
3.Redis启动.(使用绝对路径,也可以使用相对路径)
启动Redis的指令`/usr/local/bin/redis-server /etc/redis.conf`
当然直接`redis-server /etc/redis.conf`也是可以的 , 因为redis默认是配置了环境变量
![](assest/{3E2E0CA7-B0F1-4BF1-8A4C-09514F894E5C}.png)查看是否启动成功
![](assest/Pasted%20image%2020241010210619.png)
5.用客户端访问: redis-cli(也是配置了环境变量,可以直接使用)
6.修改端口
`redis-cli -p 6379`
7.redis的关闭
* 单实例关闭: redis-cli shutdown
![](assest/Pasted%20image%2020241010211205.png)
* 多实例关闭 , 指定端口关闭 `redis-cli -p 6379 shutdown`
* 进入redis再关闭
# Redis指令
指令文档: http://redis.cn/commands.html
**基础操作**
1.`set key value` 设置key-value值
2.`get key` 查询key对应的value值
3.`clear` 清屏
4.`quit/exit` 退出客户端(redis服务并没有退出)
5.`help 命令名称` 获取命令帮助文档

**对key(键)操作**
1.`keys *`: 查看当前库中所有的key
2.`exists key`: 判断某个key是否存在
3.`type key` : 查看key是什么类型
4.del key : 删除指定的key数据(对应的值也会删掉),阻塞式(删掉才能进行下一步操作)
5.unlink key :根据value选择非阻塞删除([[仅将keys从keysapce元数据中删除,实际的删除会在后续异步操作]])
6.`expire key 10` : 10秒钟: 为给定的key设置过期时间(非常有用)
7.`ttl key` 查看对应的key-value有多少秒过期(非常有用) `-1`表示永久不过期 , `-2`表示已过期

**对DB(数据库)操作**
1.`select`: 命令切换数据库---redis安装后,默认有16个库 , 0-15,默认操作的是redis的0号库
2.`dbsize`:查看当前数据库的key的数量
3.`flushdb`: 清空当前库
4.`flushall`:清空全部库
# Redis五大数据类型

