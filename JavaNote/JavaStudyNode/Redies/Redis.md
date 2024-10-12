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
## Redis五大数据类型的简介
**引言**
redis自身是一个Map , 其中所有的数据都是采用 key:value的形式存储的, 
key是字符串, value是数据 , 数据支持多种类型/结构

**redis五种常用的数据结构**
1.string
2.hash
3.list
4.set
5.sorted_set
## String常用指令
1.`set <key><value>`  添加键值对

2.`get <key>` 查询对应键值`

3.`append key value`将给定的`<value>` 追加到原值的末尾

4.`strlen key` 获得值的长度

5.`setnx key value`  只有在key 不存在时设置key 的值 

6.`incr key` 将key中存储的数字值(字符串)增1 , 只能对数字值操作 , 如果为空 , 新增值为1 

7.`decr key` 将key中存储的数字值(字符串)减1 , 只能对数字值操作 , 如果为空 , 新增值为-1

8.`incrby/decrby key 步长` 将key中存储的数字值增减. 自定义步长

9.`mset key1 value1 key2 value2 ...` 同时设置一个或多个`key-value`对

10.`mget key1 key2 key3...`同时获取一个或多个value

11.`msetnx key1 value1 key2 value2 ...` 同时设置一个或多个`key-value`对 , 当且仅当所有给定key都不存在 , 原子性 , 有一个失败则都失败

12.`getrange key 起始位置 结束位置` 获得值的范围 , 类似java中的substring 起始位置和终止位置是闭区间

13.`setrange key 起始位置 value` 用`<value>`覆写`<key>`所存储的字符串值 , 从`<起始位置>`开始(索引从0开始)

14.`setex key 过期时间 value` 设置键值的同时 , 设置过期时间 , 单位秒

15.`getset key value` , 以新换旧 , 设置了新值同时获取了旧值
## list
**简介**: list类型 , 保存多个数据 , 底层使用双向链表存储结构实现

**list存储结构示意图**
双向链表示意图
![](assest/Pasted%20image%2020241011195724.png)
1.Redis列表式简单的字符串列表 , 按照插入顺序排序 . 可以添加一个元素到列表的头部(左边)或者尾部(右边)
2.底层是一个双链表 , 对两端的操作性高 , 通过索引下标的操作中间的节点性能差

**commend**
`lpush/rpush key value value2 value3` 从左边/右边插入一个或者多个值

`lpop/rpop key` 从左边/右边吐出一个值

`rpoplpush key1 key2` 从key1列表右边出队 , key2列表左边入队

`lrange key start stop` 按照索引下标获取元素(从左到右) 可negative numbers indicating offsets sart

`lindex key index`按照索引下标获得元素(从左到右)

`llen key`获得列表长度

`linsert key before value newvalue`在value的前面插入newvalue

`lrem key n value`从左边删除n个value(从左到右)

`lset key index value`将列表key下标为index的值替换成value
## set
set提供的功能和list类似是一个列表的功能 , 特殊之处在于set是可以自动去重 , 即值是不允许重复的.
**set指令操作示意图**
![](assest/Pasted%20image%2020241011211236.png)

**command**
1.`sadd key value1 value2` 将一个或多个元素加入集合key中 , 已经存在的元素会被忽略

`smembers key` 取出集合所有值.

`sismember key value` 判断集合 key 是否含有value , 有 1 , 无 0

`sard key` 返回该集合的元素个数

`srem key value1 value2` 删除key集合中的多个元素

`spop key` 随机从集合中取出元素,取出的值会被删除

`srandmember key n`随机从该集合中读取出n个值 , 不会从集合中删除

`smove source destination value`把集合中一个值从一个集合移动到另一个集合

`sinter key1 key2`返回两个集合的交集元素.

`sunion key1 key2` 返回两集合并集的元素

`sdiff key1 key2` 返回两个集合的差集元素(key1 中的，不包含key2 中的)
## hash
**简介**
Redis hash 是一个键值对集合 , hash适合用于村塾对象 , 类似java里面`Map<String,Object>`

**存储结构示意图**
![](assest/Pasted%20image%2020241012090253.png)

**command**
`hset hash1 field value` 给hash1中的fied键赋值value

`hget hash1 field` 从hash1集合中取出field对应的值 

`hmset hash1 field1 value1 field2 value2` 批量设置hash1的值

`hmget hash1 field1 field2`批量取出hash1所对应的值

`hexists hash1 field` 查看hash1中 , field键是否存在(1表示真,0表示假)

`hkeys hash1` 列出该hash1集合的所有field(键值)

`hvals hash1` 列出该hash1集合的所有value

`hincrby hash1 field increment` 给hash1表中的field键对应的值加上1

`hsetnx hash1 field value` 将哈希表hash1 中field对应的值设为value(field键不存在)
## 有序集合 Zset(sorted set)
**简写**
1.Redis有序集合zset与普通集合set非常相似 , 是一个没有重复元素的字符串集合.
2.zset与set不同 , zset中的每个元素除了自身的值 , 还带有**权重(score)**(一般是浮点数). 有序集合中的元素是根据他们的分数进行排序的. Redis会根据分数自动维护元素的顺序.
3.能够快速的获取一个范围内的元素
4.访问有序集合的中间元素也是非常快的. 可以把有序集合作为一个没有重复成员的列表

**有序集合示意图**
![](assest/Pasted%20image%2020241012103225.png)
`zadd set1 score1 value1 score2 value2` 将一个或者多个元素加入到有序集合中

`zrange set1 start stop [WITHSCORES]` 返回有序集set1中 , 返回start-stop之间的元素 , 如果带WITHSCORES , 可以让分数一起和值返回到结果集start和stop是下标 , 从0开始

`zscore set1 value1` 返回有序集中 , 成员value1的score值

`zrangebyscore set1 min max [withscores] [limit offset count]` 返回有序集set1中 , 所有score值介于min和max之间(包括等于min或max)的成员. 有序集成员按score值递增(==从小到大==)次序排列 -- min和max等于score

`zrangebyscore set1 max min [withscores] [limit offset count]` 同上 , 不同之处是从大到小排列

`zincrby set1 increment value` 为元素的score加上增量 increment是增量

`zrem set1 value` 删除该集合下指定值的元素

`zcount set1 min max` 统计该集合 , 分数(score)区间内的元素个数

`zrank set1 value` 返回该值在集合中的排名 , 从0开始