## 1逻辑架构

##### 逻辑架构剖析(运行流程)
MySQL处理一个请求的过程
![](assest/Pasted%20image%2020240713203542.png)
==MySQL官方系统结构图==
![](assest/Pasted%20image%2020240713203609.png)
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
![](assest/Pasted%20image%2020240713210332.png)不详细
![](assest/Pasted%20image%2020240713210348.png) 详细

##### 数据库缓冲池
数据库缓冲池（Database Buffer Pool）是数据库管理系统（DBMS）中用于==临时存储数据的内存区域==。它的主要目的是提高数据库系统的性能，通过减少直接从磁盘读取和写入数据的次数来加速数据访问。
![](assest/Pasted%20image%2020240713211027.png)
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
![](assest/Pasted%20image%2020240713215417.png)

## 3索引的数据结构
### 索引的概述以及优缺点
#### 索引的概述
索引定义:索引（Index）是帮助MySQL高效获取数据的==数据结构==
**索引的优点**:1.提高数据检索的效率，降低数据库的IO成本  2.通过创建唯一索引，可以保证数据库表中每一行数据的唯一性。3. 加速表和表之间的连接,换句话说，对于有依赖关系的子表和父表联合查询时，可以提高查询速度。4.在使用分组和排序子句进行数据查询时，可以显著减少查询中分组和排序的时间，降低了CPU的消耗。
**索引的缺点**:（1）创建索引和维护索引要耗费时间，并且随着数据量的增加，所耗费的时间也会增加。（2）索引需要占磁盘空间 (3)降低更新表的速度,当对数据进行增加,删除,和修改时,索引也要动态的进行维护,这样就降低了数据的维护速度
**提示:在突发插入频繁的情况下，由于索引可以提高查询的速度，但是会影响插入记录的速度。这 种情况下，可以先先删除表中的索引，然后插入数据，插入完成后再创建索引。**
#### 聚簇索引
所有的用户记录都存在了叶子节点，数据即索引，索引即数据
**特点**
1.使用主键值的大小进行记录和页的排序
* 页内的记录是按照主键的大小顺序排成一个单向链表
* 存放用户记录的**页**也是根据页中用户记录的主键大小顺序排成一个双向链表
2.B+树的叶子节点存储的是完整的用户记录。就是指这个记录中存储了所有列的值
**优点**
* 数据访问更快, 因为聚簇索引将索引和数据保存在同一个B+树中, 减少io次数,因此从聚簇索引中获取数据比非聚簇索引更快
* 聚簇索引对于主键的排序查找和 范围查找速度非常快. 由于数据都是紧密相连,数据库不用从多个数据块中提取数据, 减少io次数
**缺点**
* 插入速度严重依赖插入顺序 , 按照主键的顺序插入是最快的方式, 否则会出现页分裂, 严重影响性能, 因此表要有自增列的id为主键
* 更新**主键**的代价很高, 更新会破坏原有的数据结构和物理存储, 一般把主键定义为不可更新
* 二级索引访问需要两次索引查找，第一次找到主键值，第二次根据主键值找到行数据
#### 二级索引(辅助索引,非聚簇索引)
聚簇索引只能在搜索条件为主键值时才能发挥作用, B+树中的数据是按主键值排序的, 如果以其它列作为搜索条件该怎么办.
答案是多建几颗B+树
![](assest/Pasted%20image%2020240812162536.png)
[c2列](#^d642d4)的解析
**回表**:根据这个以c2列大小排序的B+树只能确定要查找记录的主键值,如果想要找出实际还要根据找出的主键值,到聚簇索引中再查一遍, 这个过程称为回表
#### 联合索引
可以同时以多个列的大小作为排序规则，也就是同时为多个列建立索引，比方说我们想让B+树按照c2和c3列的大小进行排序建立.  注意,本质就是二级索引
![](assest/Pasted%20image%2020240812164328.png)
### InnoDB中的索引
#### InnoDB中的页
首先InnoDB的存储数据的方式是以页的形式存储的,并且一页是16kb ^d642d4
```
mysql> CREATE TABLE index_demo(
-> c1 INT,
-> c2 INT,
-> c3 CHAR(1),
-> PRIMARY KEY(c1)
-> ) ROW_FORMAT = Compact; c1列为主键
```
内部行格式示意图
![](assest/Pasted%20image%2020240714100549.png)
==record_type== ：记录头信息的一项属性，表示记录的类型， 0 表示普通记录、2 表示最小记
录、3 表示最大记录 , 1.目录项记录
==next_record== ：表明下一条记录是谁。
==各个列的值==：这里只记录在index_demo 表中的三个列，分别是c1 、c2 和c3 。
==其他信息==：除了上述3种信息以外的所有信息，包括其他**隐藏列的值**以及记录的额外信息。 

一些记录(不是表)放在页中是这样的
![](assest/Pasted%20image%2020240714101048.png)

#### InnoDB的数据的查找
```
SELECT [列名列表] FROM 表名 WHERE 列名 = xxx;
```
1. 在一页中的查找
1.有主键的情况下,使用二分法快速查找
2.没有索引的情况下,暴力遍历

2. 在多页中的查找
1.定位到记录所在的页
2.从所在的页内中查找相应的记录。
如果没有索引,那么我们要从第一页沿着链表一直找,会非常耗时

#### InnoDB中索引的建立过程
1. 建立目录项,目录项组合在一起,形成目录页
1.目录页的各列的值记录一页中最小的主键值
2.页号,page_no, 如下
![](assest/Pasted%20image%2020240714103433.png)
随着数据不断的增多,最后会形成B+Tree的形式
![](assest/Pasted%20image%2020240714104048.png)
假设所有存放用户记录的叶子节点代表的数据页可以存放100条用户记录，所有存放目录项记录的内节点代表的数据页可以存放1000条目录项记录，
那么：如果B+树只有1层，也就是只有1个用于存放用户记录的节点，最多能存放100 条记录。
	   如果B+树有2层，最多能存放1000×100=10,0000 条记录。
	   如果B+树有3层，最多能存放1000×1000×100=1,0000,0000 条记录。
	   如果B+树有4层，最多能存放1000×1000×1000×100=1000,0000,0000 条记录。
所以一般不会超过四层

#### InnoDB的B+树索引的注意事项
**1.跟页面位置万年不动(由上而下)**
![](assest/Pasted%20image%2020240812101730.png)

**2.内节点中目录项记录的唯一性**
内节点指非叶子节点
![](assest/Pasted%20image%2020240812104617.png)![](assest/Pasted%20image%2020240812105228.png)
![](assest/Pasted%20image%2020240812105423.png)
所以要建议把主键值也添加到二级索引内节点中的目录项记录中, 保证B+树每一层节点中各目录项记录除页号这个字段外是唯一的, 根据上面的实例, 如下是合法的有效的二级索引示意图
![](assest/Pasted%20image%2020240812110229.png)
**3.一个页面最少存2条记录**
### MyISAM中的索引方案
#### MyISAM索引的原理
InnoDB中索引即数据, 就是聚簇索引的B+树的叶子节点已经包含了用户记录, 而MyISAM的索引方案虽然是树形结构, 但是却将索引和数据分开存储, MyISAM的叶子节点存储的是主键值+数据记录地址
![](assest/Pasted%20image%2020240812154651.png)
同样也是一棵B+Tree，data域保存数据记录的地址。MyISAM中索引检索的算法为:首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其data域的值，然后以data域的值为地址, 然后进行io操作
#### MyISAM与InnoDB对比
 1.在InnoDB存储引擎中，只需要根据主键值对聚簇索引进行一次查找就能找到对应的记录，而在MyISAM 中需要进行一次回表操作(根据地址查找存记录的表)，所以MyISAM中建立的索引相当于全部都是二级索引。
 2.InnoDB数据和索引在同一文件, MyISAM索引和数据文件分离
 3.MyISAM的回表操作非常快速, InnoDB的回表操作需要再进行搜索,比较慢
 4.InnoDB要求表必须有主键（ MyISAM可以没有）。如果没有显式指定，则MySQL系统会自动选择一个可以非空且唯一标识数据记录的列(已存在)作为主键。如果不存在这种列，则MySQL自动为InnoDB表生成一个隐含字段作为主键，这个字段长度为6个字节，类型为长整型。 
### 索引的代价
索引它在时间和空间上都会有损耗
**空间上的代价**
每建立一个索引, 都要建立一个B+树, B+树的每一个节点都是数据页, 这样一来,一棵树就需要大片的内存空间
**时间上的代价**
每次对表中的数据进行增、删、改操作时，都需要去修改各个B+树索引. 因为节点的排序是有序的,增、删、改操作可能会对节点和记录的排序造成破坏，所以存储引擎需要额外的时间进行一些记录移位， 页面分裂、页面回收等操作来维护好节点和记录的排序。如果我们建了许多索引，每个索引对应的B+树都要进行相关的维护操作，会给性能拖后腿
### MySQL数据结构选择的合理性
一般,数据结构选择的标准就是磁盘i/o操作次数
#### Hash结构
![](assest/Pasted%20image%2020240812191410.png)
![](assest/Pasted%20image%2020240812191957.png)
![](assest/Pasted%20image%2020240812192024.png)
**在上图的映射过程中, 哈希函数h有可能将两个不同的关键字映射相同的位置, 叫做碰撞 , 数据库中一般采用链接法解决 , 如下图
![](assest/Pasted%20image%2020240812192216.png)**Hash结构不作为索引结构的原因**
* Hash结构如此高效的功能,仅次于查找功能,如果进行返回查询, 就会变成O(n)
* 如果需要进行排序 , Hash是没用顺序的, 所以还要进行重新排序
* 索引列的重复值如果很多, 效率会降低, 这是因为遇到Hash冲突时, 需要遍历桶中的指针进行比较, 查找关键字, 非常耗时
![](assest/Pasted%20image%2020240812193709.png)
Hash索引的适应性
InnoDB本身是不支持Hash索引的, 但是提供自适应Hash索引. 只有在某个数据被经常访问, 当满足条件的时候, 就将这个数据页的地址存放到临时Hash表中![](assest/Pasted%20image%2020240812194357.png)
采用自适应Hash索引目的是为方便根据sql的查询条件加速定位到叶子节点, 特别是B+树层次比较深的时候
可以通过这条指令
`mysql> show variables like '%adaptive_hash_index';` 查看InnoDB是否开启了自适应Hash
#### 二叉搜索树
首先,磁盘的IO次数和索引树的高度是相关的
**二叉搜索树的特点**
* 一个节点只能有两个子节点, 也就是一个节点度不能超过2
* 左子节点<本节点; 右子节点>=本节点.
有极端的情况, 插入的顺序是单调性的, 性能上就会退化成一条链表 
![](assest/Pasted%20image%2020240812202209.png)
为了提高查询效率就需要减少磁盘IO数, 为了减少磁盘IO的次数, 就需要降低树的高度, 因此也就出现了AVL树
#### AVL树
解决了二叉树退化成链表的问题 , AVL树(平衡二叉搜索树) , 它在二叉搜索树的基础上增加了约束 
**它是一颗空树或者它的左右两个子树的高度差的绝对值不超过1 , 并且左右两个子树都是一颗平衡二叉树**
平衡二叉树包括了平衡二叉搜索树, 红黑树, 数堆, 伸展树 
当n比较大的时候,深度也会变的非常的大
![](assest/Pasted%20image%2020240812203337.png)
![](assest/Pasted%20image%2020240812203403.png)
![](assest/Pasted%20image%2020240812203412.png)
树从瘦高变为矮胖 , 性能就会提高
#### B-Tree
B树的英文是Balance Tree, 也就是多路平衡查找树. 简写为B-Tree. 它是非常的矮胖的
B树的结构
![](assest/Pasted%20image%2020240812205720.png)
B树的每一个节点, 最多包含m个子节点 , m称为B树的阶. 
#### B+Tree
**B+Tree的介绍**
B+Tree是一种多路搜索树, 基于BTree做了改进, 主流的DBMS都支持B+数的索引方式, 比如MySQL
**B树和B+树的区别**
1.有k个孩子的节点就是k个关键字, 也就是孩子数量=关键字数, 而B树中, 孩子数量=关键字数+1
2.非叶子结点的关键字也会同时保存在叶子节点中 , 并且是在子叶子节点
3.非叶子节点仅用于索引, 不保存数据记录,根据,根据记录有关的信息都放在叶子节点中. 而 B 树中， 非叶子节点既保存索引，也保存数据记录。
4.所有关键字都在叶子节点出现，叶子节点构成一个有序链表，而且叶子节点本身按照关键字的大小从小到大顺序链接。
## 4InnoDB数据存储结构
### 数据库的存储结构:页
**页的简介**
页是磁盘和内存==交互基本单位==默认大小事16kb. 也就是说一次最少从磁盘中获取16kb的内容到内存中,一次最少把内存16kb内容刷新到磁盘中
记录是按照行来存储的，但是数据库的读取并不以行为单位，否则一次读取（也就是一次I/O操作)只能处理一行数据，效率会非常低。

**页结构概述**
页不在物理结构上相连, 通过双向链表相连 , 每个数据页中的记录会按照主键值从小到大的顺序组成一个`单向链表`,每个数据页都会为存储在它里边的记录生成一个`页目录`在通过主键查找某条记录的时候可以在页目录`中使用二分法`快速定位到对应的槽，然后再遍历该槽对应分组中的记录即可快速找到指定的记录。(下列是叶子节点)
![](assest/Pasted%20image%2020240814150707.png)

**页的大小**
不同的数据库管理系统（简称DBMS）的页大小不同。比如在MySQL的InnoDB存储引擎中，默认页的大小是**16KB**，可以通过下面的命令来进行查看:
```sql
show variables like '%innodb_page_size%';
/*
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| innodb_page_size | 16384 |
+------------------+-------+
*/
```

**页的上层结构**
另外在数据库中，还存在区（Extent)、段(Segment)和表空间（Tablespace)的概念。行、页、区、段、表空间的关系如下图所示:
![](assest/Pasted%20image%2020240814154111.png)
区(Extent)是比页大一级的存储结构，在InnoDB存储引擎中，一个区会分配`64个连续的页`。因为InnoDB中的页大小默认是16KB，所以一个区的大小是`64*16KB= 1MB。`
段(Segment)由一个或多个区组成，区在文件系统是一个连续分配的空间（在InnoDB中是连续的64个页)，不过在段中不要求区与区之间是相邻的。`段是数据库中的分配单位，不同类型的数据库对象以不同的段形式存在。` 当创建数据表、索引的时候，就会相应创建对应的段，比如创建一张表时会创建一个表段，创建一个索引时会创建一个索引段。
表空间（Tablespace)是一个逻辑容器，表空间存储的对象是段，在一个表空间中可以有一个或多个段，但是一个段只能属于一个表空间。数据库由一个或多个表空间组成，表空间从管理上可以划分为系统表空间，`用户表空间`、`撤销表空间`、`临时表空间`等。
### 页的内部结构
**页的简介**
页,可分为数据页(保存B+树节点) , 系统页 , Undo页 和 事务数据页等. 数据页是我们最常使用的页. 16kb的页可分为7个部分. 分别是文件头(File Header), 页头(Page Header) , 最大最小记录(Infimum+supremum)、用户记录(User Records)、空闲空间(Free Space)、页目录(Page Directory)和文件尾(File Tailer)
![](assest/Pasted%20image%2020240814161924.png)
这7个部分的作用如下表
![](assest/Pasted%20image%2020240814162731.png)

**File Header and File Trailer**
![](assest/Pasted%20image%2020240814180350.png)
**从数据页角度看B+树如何查询**
B+树按照节点分为两部分: 叶子节点 , 存储记录 2.非叶子节点, 节点高度大于0, 存储索引和页面指针
![](assest/Pasted%20image%2020240814200230.png)
通过页结构去理解B+树的结构, 进而理解索引进行检索的原理
* B+树是如何进行记录检索的?
	通过B+树的索引查询行记录，首先是从B+树的根开始，逐层检索，直到找到叶子节点，也就是找到对应的数据页为止，将数据页加载到内存中，页目录中的槽(slot)采用`二分查找`的方式先找到一个粗略的记录分组然后再在分组中通过`链表遍历`的方式查找记录。
* 普通索引和唯一索引在查询效率上有什么不同
	唯一索引就是在普通索引上增加了约束性，也就是关键字唯一，找到了关键字就停止检索。而普通索引，可能会存在用户记录中的关键字相同的情况，根据页结构的原理，当我们读取一条记录的时候，不是单独将这条记录从磁盘中读出去，而是将这个记录所在的页加载到内存中进行读取。InnoDB存储引擎的页大小为16KB，在一个页中可能存储着上千个记录，因此在普通索引的字段上进行查找也就是在内存中多几次“`判断下一条记录`”的操作，对于CPU来说，这些操作所消耗的时间是可以忽略不计的。所以对一个索引字段进行检索，采用普通索引还是唯一索引在检索效率上基本上没有差别。
### InnoDB行格式(记录格式)
在MySQL应用层 , 平时的数据以行行为单位向表中插入数据 , 这些记录在磁盘上的存放方式也被称为==行==格式或者记录格式
InnoDB存储引擎设计了4种不同类型的行格式, 分别是Compact(紧密) , Redundant(冗余), Dynamic(动态)和Compressed(压缩)行格式.
查看MySQL的默认行格式
```sql
mysql> select @@innodb_default_row_format;
+-----------------------------+
| @@innodb_default_row_format |
+-----------------------------+
| dynamic                     |
+-----------------------------+
1 row in set (0.00 sec)

# 查询单张表行格式
mysql> show table status like 'departments' \G
*************************** 1. row ***************************
           Name: departments
         Engine: InnoDB
        Version: 10
 #行格式  Row_format: Dynamic
           Rows: 27
 Avg_row_length: 606
    Data_length: 16384
Max_data_length: 0
   Index_length: 49152
      Data_free: 0
 Auto_increment: NULL
    Create_time: 2022-03-23 14:56:38
    Update_time: 2022-03-23 14:56:38
     Check_time: NULL
      Collation: utf8_general_ci
       Checksum: NULL
 Create_options:
        Comment:
1 row in set (0.01 sec)
```
#### 指定行格式的语法
在创建或修改表的语句中指定行格式：
CREATE TABLE 表名 (列的信息) ROW_FORMAT=行格式名称
ALTER TABLE 表名 ROW_FORMAT=行格式名称
```sql
mysql> CREATE TABLE record_test_table (
    ->     col1 VARCHAR(8),
    ->     col2 VARCHAR(8) NOT NULL,
    ->     col3 CHAR(8),
    ->     col4 VARCHAR(8)
    -> ) CHARSET=ascii ROW_FORMAT=COMPACT;
```
重点是`row_format=compact`
#### COMPACT行格式
**COMPACT行格式简介**
在MySQL 5.1版本中，默认设置为Compact行格式。一条完整的记录其实可以被分为记录的额外信息和记录的真实数据两大部分。
![](assest/Pasted%20image%2020240814211126.png)
**变长字段列表**
MySQL支持一些变长的数据类型，比如VARCHAR(M)、VARBINARY(M)、TEXT类型，BLOB类型，这些数据类型修饰列称为变长字段，变长字段中存储多少字节的数据不是固定的，所以我们在存储真实数据,还要把这些数据占用的字节数也存起来。在Compact行格式中，把所有变长字段的真实数据占用的字节长度都存放在记录的开头部位，从而形成一个变长字段长度列表。
 
注意：这里面存储的变长长度和字段顺序是反过来的。比如两个varchar字段在表结构的顺序是a(10)，b(15)。那么在变长字段长度列表中存储的长度顺序就是15，10，是反过来的。
 
以record_test_table表中的第一条记录举例：因为record_test_table表的col1、col2、col4列都是VARCHAR(8)类型的，所以这三个列的值的长度都需要保存在记录开头处，注意record_test_table表中的各个列都使用的是ascii字符集（每个字符只需要1个字节来进行编码）。
![](assest/Pasted%20image%2020240814204953.png)
又因为这些长度值需要按照列的逆序存放，所以最后变长字段长度列表的字节串用十六进制表示的效果就是（各个字节之间实际上没有空格，用空格隔开只是方便理解）：
06 04 08  
把这个字节串组成的变长字段长度列表填入上边的示意图中的效果就是：
![](assest/Pasted%20image%2020240814204319.png)

**NULL值列表**
Compact行格式会把(可以为NULL的列)(没有not null字段)统一管理起来，存在一个标记为NULL值列表中。如果表中没有允许存储 NULL 的列，则 NULL值列表也不存在了。
为什么定义NULL值列表？
之所以要存储NULL是因为数据都是需要对齐的，如果没有标注出来NULL值的位置，就有可能在查询数据的时候出现混乱。如果使用一个特定的符号放到相应的数据位表示空置的话，虽然能达到效果，但是这样很浪费空间，所以直接就在行数据得头部开辟出一块空间专门用来记录该行数据哪些是非空数据，哪些是空数据，格式如下：
 
1. 二进制位的值为1时，代表该列的值为NULL。
2. 二进制位的值为0时，代表该列的值不为NULL。
 
例如：字段 a、b、c，其中a是主键，在某一行中存储的数依次是 a=1、b=null、c=2。那么Compact行格式中的NULL值列表中存储：01。第一个0表示c不为null，第二个1表示b是null。这里之所以没有a是因为数据库会自动跳过主键，因为主键肯定是非NULL且唯一的，在NULL值列表的数据中就会自动跳过主键。
 
record_test_table的两条记录的NULL值列表就如下：
 
第一条记录：
![](assest/Pasted%20image%2020240814210327.png)
第二条记录：
![](assest/Pasted%20image%2020240814210349.png)

**记录头信息**
表中记录的行格式示意图
![](assest/Pasted%20image%2020240815124232.png)
### 区,段与碎片区
#### 为什么要有区
`B+`树的每一层中的页都会形成一个双向链表，如果是以`页为单位`来分配存储空间的话，双向链表相邻的两个页之间的`物理位置`可能离得非常远。我们介绍B+树索引的适用场景的时候特别提到范围查询只需要定位到最左边的记录和最右边的记录,然后沿着双向链表一直扫描就可以了，而**如果链表中相邻的两个页物理位置离得非常远，就是所谓的`随机I/0`。再一次强调，磁盘的速度和内存的速度差了好几个数量级，`随机I/0是非常慢的`**，所以我们**应该尽量让链表中相邻的页的物理位置也相邻**，这样进行范围查询的时候才可以使用所谓的`顺序I/0`
==这本质是应用了磁盘的预读特性==
引入`区`的概念，一个区就是在物理位置上**连续**的`64个页`。因为InnoDB 中的页大小默认是16KB，所以一个区的大小是`64*16KB=`1MB。在表中数据量大的时候，为某个索引分配空间的时候就不再按照页为单位分配了，而是按照`区为单位`分配，甚至在表中的数据特别多的时候，可以一次性分配多个连续的区。虽然可能造成`一点点空间的浪费`（数据不足以填充满整个区)，但是从性能角度看，可以消除很多的随机I/O，==功大于过!== 这里是连续的64个页, 但是具体的两个页之间还是用指针相连的, 保证一大块区域连续
#### 为什么要有段
对于**范围查询**，其实是对B+树叶子节点中的记录进行顺序扫描，而如果不区分叶子节点和非叶子节点，统统把节点代表的页面放到申请到的区中的话，进行范围扫描的效果就大打折扣了。所以InnoDB对B+树的`叶子节点`和`非叶子节点`进行了区别对待，也就是说叶子节点有自己独有的区，非叶子节点也有自己独有的区。存放叶子节点的区的集合就算是一个`段( segment)`，存放非叶子节点的区的集合也算是一个段。也就是说一个索引会生成2个段，一个`叶子节点段`，一个`非叶子节点段`。

除了索引的叶子节点段和非叶子节点段之外，InnoDB中还有为存储一些特殊的数据而定义的段，比如回滚段。所以，常见的段有==`数据段`、`索引段`、`回滚段`==。数据段即为B+树的叶子节点，索引段即为B+树的非叶子节点。

在InnoDB存储引擎中，对段的管理都是由引擎自身所完成，DBA不能也没有必要对其进行控制。这从一定程度上简化了DBA对于段的管理。

**段其实不对应表空间中某一个连续的物理区域，而是一个逻辑上的概念**，由若干个零散的页面以及一些完整的区组成。
#### 为什么要有碎片区
默认情况下，一个使用InnoDB存储引擎的表只有一个聚簇索引，一个索引会生成2个段，而段是以区为单位申请存储空间的，一个区默认占用1M `(64*16Kb=1024Kb）`存储空间，所以**默认情况下一个只存了几条记录的小表也需要2M的存储空间么?以后每次添加一个索引都要多申请2M的存储空间么?这对于存储记录比较少的表简直是天大的浪费。这个问题的症结在于到现在为止我们介绍的区都是非常纯粹的**，也就是一个区被整个分配给某一个段，或者说区中的所有页面都是为了存储同一个段的数据而存在的，即使段的数据填不满区中所有的页面，那余下的页面也不能挪作他用。
目的是为了尽可能的利用空闲空间
为了考虑以完整的区为单位分配给某个段对于`数据量较小`的表太浪费存储空间的这种情况，InnoDB提出了一个`碎片(fragment)区`的概念。在一个碎片区中，==并不是所有的页都是为了存储同一个段的数据而存在的==，而是碎片区中的页可以用于不同的目的，比如有些页用于段A，有些页用于段B，有些页甚至哪个段都不属于。`碎片区直属于表空间`，并不属于任何一个段。==只属于表空间的区==

所以此后为某个段分配存储空间的策略是这样的:

- 在刚开始向表中插入数据的时候，段是从某个碎片区以单个页面为单位来分配存储空间的
- 当某个段已经占用了`32个碎片区`页面之后，就会申请以完整的区为单位来分配存储空间。

所以现在段不能仅定义为是某些区的集合，更精确的应该是**某些零散的页面**以及**一些完整的区**的集合。
#### 区的分类
区大体上可以分为4种类型:

- `空闲的区(FREE)`: 现在还没有用到这个**区**中的任何**页**面。
- `有剩余空间的碎片区(FREE_FRAG):` 表示**碎片区**中还有可用的**页**面。
- `没有剩余空间的碎片区(FULL_FRAG)`︰表示碎片区中的所有页面都被使用，没有空闲页面。
- `附属于某个段的区(FSEG):`每一个索引都可以分为叶子节点段和非叶子节点段。

处于`FREE`、`FREE_FRAG`以及`FULL_FRAG`这三种状态的区都是独立的，直属于表空间。而处于FSEG状态的区是附属于某个段的。

> 如果把表空间比作是一个集团军，段就相当于师，区就相当于团。一般的团都是隶属于某个师的，就像是处于`FSEG`的区全都隶属于某个段，而处于`FREE`、`FREE_FRAG`以及`FULL_FRAG`这三种状态的区却直接隶属于表空间，就像独立团直接听命于军部一样。
### 表空间
表空间可以看做是InnoDB存储引擎逻辑结构的最高层，所有的数据都存放在表空间中。
表空间是一个`逻辑容器`，表空间存储的对象是段，在一个表空间中可以有一个或多个段，但是一个段只能属于一个表空间。**表空间数据库由一个或多个表空间组成**，表空间从**管理**上可以划分为`系统表空间` (System tablespace)、`独立表空间`(File-per-table tablespace)、`撤销表空间`(Undo Tablespace)和`临时表空间`(Temporary Tablespace）等。

**1.独立表空间**
* 独立表空间，即每张表有一个独立的表空间，也就是数据和索引信息都会保存在自己的表空间中。独立的表空间(即:单表)可以在不同的数据库之间进行`迁移`。
* ==空间可以回收(DROPTABLE操作可自动回收==表空间;其他情况，表空间不能自己回收)。如果对于统计分析或是日志表，删除大量数据后可以通过: `alter table TableName engine=innodb`;回收不用的空间。对于使用独立表空间的表，不管怎么删除，表空间的碎片不会太严重的影响性能，而且还有机会处理
* 独立表空间结构:独立表空间由段, 区, 页组成. 
* 真实表空间的大小: 我们到数据目录里看，会发现一个新建的表对应的`.ibd`文件只占用了`96K`，才6个页面大小(MySQL5.7中)，这是因为一开始表空间占用的空间很小，因为表里边都没有数据。不过别忘了这些.ibd文件是`自扩展的`，随着表中数据的增多，表空间对应的文件也逐渐增大。
* 查看InnoDB的表空间类型
```sql
# 查看是否独立表空间
mysql> show variables like 'innodb_file_per_table';
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_file_per_table | ON    |
+-----------------------+-------+
1 row in set, 1 warning (0.00 sec)
```
MySQL8.0中7个页面大小, 原因.idb还存了表结构. 

**2.系统表空间**
系统表空间的结构和独立表空间基本类似，只不过由于整个MySQL进程只有一个系统表空间，在系统表空间中会额外记录一些有关整个系统信息的页面，这部分是独立表空间中没有的。

**3.表空间在mysql中的实际使用**
每当我们向一个表中插入一条记录的时候，`MySQL校验过程`如下:
先要校验一下插入语句对应的表存不存在，插入的列和表中的列是否符合，如果语法没有问题的话，还需要知道该表的聚簇索引和所有二级索引对应的根页面是哪个表空间的哪个页面，然后把记录插入对应索引的B+树中。所以说，MySQL除了保存着我们插入的用户数据之外，还需要保存许多额外的信息，比方说:
## 索引的创建与设计原则
### 索引的声明以及使用
#### 索引的分类
MySQL的索引主要有**普通索引, 唯一索引 , 全文索引 , 单列索引 , 多列索引 , 和空间索引
* 从功能逻辑上说 , 索引主要有4种 , 分别是普通索引 , 唯一索引 , 主键索引 , 全文索引
* 按物理实现方式可分为:聚簇索引和非聚簇索引
* 按作用字段分: 单列索引和联合索引
空间索引
使用参数spatial可以设置索引为空间索引. 空间索引只能建立在空间数据类型上, 可以提高获取空间数据的效率. 空间数据类型包括GEONETRY(几何学)、POINT、LINESTRING(线串)和POLYGON(多边形)等
全文索引
全文索引(全文检索) 是目前搜索引擎使用的一种关键技术 , 索引能够利用分词技术等多种算法智能分析出文本文字中关键词的频率和重要性, 然后按照一定的算法规则智能的筛选出我们想要的搜索结果. 文本索引非常适合大型数据集. 数据小的优势体现不出开
用法: 使用参数FULLTEXT可以设置索引为全文索引
特点:查询数据较大的字符串类型的字段时 , 使用全文索引可以提高查询速率

**不同的存储引擎支持的索引类型不一样**
**InnoDB** ： 支持 B-tree、Full-text 等索引，不支持 Hash索引；

**MyISAM** ： 支持 B-tree、Full-text 等索引，不支持 Hash 索引；

Memory ： 支持 B-tree、Hash 等索引，不支持 Full-text 索引；

NDB ： 支持 Hash 索引，不支持 B-tree、Full-text 等索引；

Archive ： 不支持 B-tree、Hash、Full-text 等索引；
#### 创建索引
MySQL支持在单或多列上创建索引; 
创建时机 , 在创建表的定义语句`create table` 中指定索引列, 使用`ALTER TABLE`语句在存在的表上创建索引 , 或者使用`create index`语句在已存在的表上添加索引
**创建表时创建索引**
在创建表的时候 , 定义主键约束 , 外键约束 , 唯一性约束 , 都是在指定列上创建索引
example
主键索引
```sql
CREATE TABLE dept(
dept_id INT PRIMARY KEY AUTO_INCREMENT,
dept_name VARCHAR( 20 )
);
```
外键索引和唯一索引
```sql
CREATE TABLE emp(
emp_id INT PRIMARY KEY AUTO_INCREMENT,
emp_name VARCHAR( 20 ) UNIQUE,
dept_id INT,
CONSTRAINT emp_dept_id_fk FOREIGN KEY(dept_id) REFERENCES dept(dept_id)
);
```
如果想要显示创建表时创建索引 , 语法如下
```sql
CREATE TABLE table_name [col_name data_type]
[UNIQUE | FULLTEXT | SPATIAL][INDEX |KEY][index_name] (col_name [length]) [ASC | DESC]
```
- UNIQUE、FULLTEXT和SPATIAL为可选参数，分别表示唯一索引、全文索引和空间索引；
- INDEX与KEY为同义词，两者的作用相同，用来指定创建索引；
- index_name指定索引的名称，为可选参数，如果不指定，那么MySQL默认col_name为索引名；
- col_name为需要创建索引的字段列，该列必须从数据表中定义的多个列中选择；
- length为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度；
- ASC或DESC指定升序或者降序的索引值存储。

`show index from book`查看索引情况 . 重点语句

**删除索引**
在(hsp)mysqlpdf上
### MySQL8.0索引新特性
#### 支持降序索引
在MySQL4版本开始就已经支持降序索引的语法了, 但实际上该DESC定义是被忽略的(实际上索引的创建仍然是升序索引),一直到MySQL8.x版本才开始真正的支持降序索引(仅InnoDB存储引擎)
在某些场景下降序索引意义重大. 比如一个查询要求按特定查询(多个列进行排序, 且顺序不一样), 那么使用降序索引可以避免数据库使用额外的文件排序操作, 从而提高性能

模版样例
```sql
CREATE TABLE ts1(a int, b int, index idx_a_b(a asc, b desc) ) ;
```
给它创建降序索引 , 关键是`index idx_a_b(a asc,b desc)`
如果在MySQL5.7版本查看ts1表结构
```sql
mysql> show create table ts1 \G
*************************** 1. row ***************************
       Table: ts1
Create Table: CREATE TABLE `ts1` (
  `a` int(11) DEFAULT NULL,
  `b` int(11) DEFAULT NULL,
  KEY `idx_a_b` (`a`,`b`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
1 row in set (0.00 sec)
```
解析
会发现, 没有出现DESC这个字眼 , 说明KEY还是升序排序的
MySQL8.0查看表结构
```sql
mysql> show create table ts1 \G
*************************** 1. row ***************************
       Table: ts1
Create Table: CREATE TABLE `ts1` (
  `a` int DEFAULT NULL,
  `b` int DEFAULT NULL,
  KEY `idx_a_b` (`a`,`b` DESC)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3
1 row in set (0.00 sec)
```
细节:
`KEY`出现了DESC, 说明是降序排序
注意 降序索引只对查询中特定的排序顺序有效，如果使用不当，反而查询效率更低。例如，上述查询排序条件改为order by a desc, b desc，MySQL 5.7的执行计划要明显好于MySQL 8.0。
#### 隐藏索引
在MySQL5.7版本之前 , 只能通过显示的方式删除索引 . 此时 , 如果发现删除索引后出现错误 , 又只能通过显示创建索引的方式将删除的索引创建回来. 如果数据表中的数据量非常的大, 或者数据表本身比较大 , 这种操作就会消耗系统过多的资源.

从MySQL 8.x开始支持`隐藏索引（invisible indexes）`，只需要将待删除的索引设置为隐藏索引，使查询优化器不再使用这个索引（即使使用force index（强制使用索引），优化器也不会使用该索引）确认将索引设置为隐藏索引后系统不受任何响应，就可以彻底删除索引。`这种通过先将索引设置为隐藏索引，再删除索引的方式就是软删除`。

模版样例
1.创建隐藏索引
```sql
#创建方式1
create table book2(
	id int primary key,
    book_name varchar(32),
    INDEX idx_name(book_name) INVISIBLE
);
#创建方式2
ALTER TABLE book2 ADD index idx_name(book_name) INVISIBLE;
```
注意细节:
* 方式2, 要先有表
* 主键不能被设置为隐藏索引. 当表中没有显示主键时, 表中的第一个唯一非空索引会成为隐式索引, 也不能设置为隐藏索引
* 关键字是`INVISIBLE`
* 隐藏索引还是会随着内容的更新同步更新,如果一个索引长期隐藏 , 建议直接删除, 因为索引的存在会影响插入, 更新和删除的性能
2.切换索引可见状态
```sql
ALTER TABLE book2 alter index idx_name visible; # 切换成非隐藏索引
ALTER TABLE book2 alter index idx_name invisible; # 切换成非隐藏索引
```
3.使隐藏索引对查询优化器可见(查询优化器可以使用隐藏索引)
查看查询优化器的开关状态
```sql
mysql> select @@optimizer_switch \G
*************************** 1. row ***************************
@@optimizer_switch: index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=off,skip_scan=on,hash_join=on,subquery_to_derived=off,prefer_ordering_index=on,hypergraph_optimizer=off,derived_condition_pushdown=on
1 row in set (0.12 sec)
```
细节`use_invisible_indexes=off` 对隐藏索引是关的
然后打开开关
```sql
mysql> set session optimizer_switch="use_invisible_indexes=on" ;
Query OK, 0 rows affected (0.06 sec)
```
### 索引的设计原则
#### 数据准备  
创建数据库和表
```sql
CREATE DATABASE atguigudb1;
USE atguigudb1;

#1.创建学生表和课程表
CREATE TABLE `student_info` (
`id` INT( 11 ) NOT NULL AUTO_INCREMENT,
`student_id` INT NOT NULL ,
`name` VARCHAR( 20 ) DEFAULT NULL,
`course_id` INT NOT NULL ,
`class_id` INT( 11 ) DEFAULT NULL,
`create_time` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT= 1 DEFAULT CHARSET=utf8;

CREATE TABLE `course` (
`id` INT( 11 ) NOT NULL AUTO_INCREMENT,
`course_id` INT NOT NULL ,
`course_name` VARCHAR( 40 ) DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT= 1 DEFAULT CHARSET=utf8;
```
#### 创建索引的符合情况
**字段的数值有唯一性的限制**
业务上具有唯一特性的字段, 即使是组合字段 , 也必须建成唯一索引.(阿里开发手册)
主要是为了提高查找速度

**频繁作为WHERE查询条件的字段**
某个字段在SELECT语句的WHERE条件中经常被使用到 , 那么就需要给这个字段创建索引.

**经常GROUP BY和ORDER BY的列建立索引**
使用GROUP BY对数据进行分组查询, 或者使用ORDER BY对数据进行排序的时候 , 都需要**分组或者排序的字段进行索引**

**UPDATE , DELETE的WHERE条件列**
和上面频繁使用WHERE的原因是一样的

**DISTINCT字段需要创建索引**
有索引可以更高效的识别重复索引 , 和查找记录 , 如果没有索引要扫描整张表是非常耗时间的. 因为索引的是排序好的 , 所以可以更加高效的进行去重操作

**多表JOIN连接操作时, 创建索引注意事项**
* 连接的表数量不要超过三个 , 因为每增加一张表就相当于增加一套嵌套循环
* 对于连接的字段创建索引 , 要求该字段在多张表中的类型必须一致

**尽量用数据范围小的列创建索引**
* 数据类型越小，在查询时进行的比较操作越快
* 数据类型小 , 索引占用的存储空间就越小 , 单位数据页存下的索引就越多 , 从而B+树的层数就越低 , 减小磁盘I/O带来的性能损耗 , 也意味着可以更多的数据页缓存到内存中 , 从而加快读写效率

**使用字符串前缀创建索引**
通过截取字段前面的一部分内容建立索引 , 这个就叫前缀索引, 这样在查找记录时虽然不能精确的定位到记录的位置  , 到那时能定位到相应前缀所在的位置 , 然后根据前缀相同的记录的主键值回表查询完整的字符串值 , 既节约空间, 又减少了字符串的匹配时间 , 还能大体的解决排序问题
比如
```sql
create table shop(address varchar( 120 ) not null);

alter table shop add index(address( 12 ));
```
在varchar字段上建立索引时 , 必须指定索引的长度 , 没必要对全字段建立索引
索引的长度与区分度是一对矛盾体，一般对字符串类型数据，长度为 20 的索引，区分度会高达`90% 以上`，可以使用 count(distinct left(列名, 索引长度))/count(*)的区分度来确定。

**使用最频繁的列放到联合索引的左侧**
这样可以较少的建立一些索引. 同时 , 优于'最左前缀原则' , 可以增加联合索引的使用率

**在多个字段都要创建索引的情况下 , 联合索引优于单值索引**
#### 限制缩影的数目
1.每个索引都需要占用磁盘空间, 索引多意味着占用磁盘空间就大
2.索引会影响`INSERT`,`DELETE`,`UPDATE`等语句的性能, 因为索引会随着数据的更新而改变
3.如果索引过多, 会增加MySQL优化器生成执行计划的时间 , 降低查询性能. 因为优化器在选择如何优化查询时候, 会根据统一信息,对每个可以用上的索引进行评估 .

#### 哪些情况不适合创建索引

**在where中使用不到的字段 , 不要设置索引**

**数据量小的表最好不要使用索引**

**有大量重复数据的列上不要建立索引**
索引的选择性差, 查询性能提升不明显

**经常更新的表不要创建过多的索引**
索引太多 , 在更新索引的时候会造成负担 , 从而影响效率
经常更新的表意味着`update`操作比较多 , 索引虽然提高了查询速度, 但是同时却降低更新表的速度

**不建议用无序的值作为索引**

**删除不再使用或者很少用的索引**

**不要定义冗余或者重复的索引**
不要单独创建在联合索引中已有的索引
```sql
CREATE TABLE person_info(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    name VARCHAR( 100 ) NOT NULL,
    birthday DATE NOT NULL,
    phone_number CHAR( 11 ) NOT NULL,
    country varchar( 100 ) NOT NULL,
    PRIMARY KEY (id),
    KEY idx_name_birthday_phone_number (name( 10 ), birthday, phone_number),
    KEY idx_name (name( 10 ))
);		
```
这个`KEY idx_name(name(10))`就是冗余索引, 重复索引就是对已有的字段再创建一个索引
## 性能分析工具的使用
### 数据库服务器的优化步骤
整个流程分为两个部分`观察(Show status)`和`行动(Action)`两个部分,观察要使用分析工具
![](assest/Pasted%20image%2020240822104332.png)
解析上图
`存在周期性波动`有可能是周期性节点的原因, 比如双十一等促销活动, 这个可以通过A1这一步解决, 也就是加缓存 , 或者更改缓存失效的策略
S2这一步, 需要开启慢查询 , 慢查询可以定位执行慢的SQL语句 . 我们可以通过设置`long_query_time`参数定义'慢'的阈值, 如果SQL执行时间超过了`long_query_time`, 则认为是慢查询 . 
到S3这一步 , 已经知道那句SQL语句有问题 , 然后用`EXPLAIN`分析该SQL语句, 再使用`show profile`查看SQL中每一个步骤的时间成本. 这样我们可以了解SQL查询慢是因为执行时间长 , 还是等待时间长
**解决SQL等待时间过长的方法**
我们可以`调优服务器的参数` , 增加数据库缓冲池等.
**解决SQL执行时间过长**
考虑是否是索引设计的问题 , 查询关联的数据表过多 , 数据表的字段设计问题
如果上面的两种方法都不能解决 , 说明进入了性能瓶颈 , 这个时候可以考虑增加服务器 , 采用读写分离的架构 , 或者对数据库进行分库分表
![](assest/Pasted%20image%2020240822111807.png)
### 查看系统性能参数
在MySQL中, 可以使用`show status`语句查询一些MySQL数据库服务器的性能参数 , 执行频率

SHOW STATUS语句语法如下
```sql
SHOW [GLOBAL|SESSION] STATUS LIKE'参数';
```
一些常用的性能参数如下
* `connections`: 连接到MySQL服务器的次数 , 向mysql连接一次算一次
* `uptime`: MySQL服务器的上线时间 , 连接服务器(MySQL服务器)的时长(秒) .
* `slow_queries`: 慢查询的次数 , 默认10秒
- `Innodb_rows_read`：Select查询返回的行数
- `Innodb_rows_inserted`：执行INSERT操作插入的行数
- `Innodb_rows_updated`：执行UPDATE操作更新的行数
- `Innodb_rows_deleted`：执行DELETE操作删除的行数
- `Com_select`：查询操作的次数。
- `Com_insert`：插入操作的次数。对于批量插入的 INSERT 操作，只累加一次。
- `Com_update`：更新操作的次数。
- `Com_delete`：删除操作的次数。
`slow_queries`==慢查询次数参数==可以结合慢查询日志找出慢查询语句 , 然后针对慢查询语句进行`表结构优化`或者`查询语句优化`. 下面指令可以查看sql语句的指令情况
`show status like 'Innodb_rows_%';`
![](assest/Pasted%20image%2020240823203956.png)
表示在mysql整个生命周期中 , sql语句的情况 , 比如 read表示整个周期中一共查询了14条记录
### 统计SQL的查询成本: last_query_cost
一条SQL查询语句在执行前需要使用查询优化器确定查询执行计划，如果存在多种执行计划的话，MySQL会计算每个计划的执行成本，从中选择成本最小的作为一个最终执行的执行计划
我们可以通过查看会话中的`last_query_cost`变量值来得到==当前==查询的成本. 它代表着执行效率,这个成本是综合决定的, 并不是一个具体描述mysql效率的指标. 
for example
```sql
CREATE TABLE `student_info` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`student_id` INT NOT NULL ,
`name` VARCHAR(20) DEFAULT NULL, `course_id` INT NOT NULL ,
`class_id` INT(11) DEFAULT NULL,
`create_time` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```
想要查询id=10000的记录 , 然后查询成本, 查看执行效率是否合格
`SELECT student_id, class_id, NAME, create_time FROM student_info WHERE id = 10000;`
然后查询一下last_cost_query
```sql
mysql> SHOW STATUS LIKE 'last_query_cost';
+-----------------+----------+
| Variable_name | Value |
+-----------------+----------+
| Last_query_cost | 1.000000 |
+-----------------+----------+
```
发现查询id=10000的student信息需要加载一页的数据页
如果查询id在1-90000的信息 , 需要21页的数据页, 但是查询时间花销差不多

**这得到的结论是**
从页加载的角度来看1.位置决定效率. 如果页就在数据库缓冲池中 , 那么效率是最高的 , 对单个页的读取来说 , 如果页存在内存中 , 会比存在磁盘中读取效率高很多
2.`批量决定效率` . 如果从磁盘中对单一页进行随机读 , 那么效率是很低的 , 但是采用顺序读取的方式, 批量对页进行读取 , 平均一页的读取效率就会提升很多 , 甚至要快于单个页面在内存中的随机读取
上面的那种情况 , 就是顺序存放 , 查询1-90000的信息批量读取 , 平均单页消耗的时间成本极低
### 定位执行慢的SQL: 慢查询日志
MySQL的慢查询日志，用来记录在MySQL中`响应时间超过阀值`的语句，具体指运行时间超过`long_query_time`值的SQL，则会被记录到慢查询日志中。long_query_time的默认值为`10`
主要作用是让我们找到不能接受的sql语句 , 然后结合explain去分析 , 最后优化
默认情况下，MySQL数据库`没有开启慢查询日志`，需要我们手动来设置这个参数。**如果不是调优需要的话，一般不建议启动该参数**，因为开启慢查询日志会或多或少带来一定的性能影响
#### 开启慢查询日志参数
**1.查看慢查询日志状态和开启slow_query_log**
```sql
show variables like '%slow_query_log%'; 
set global slow_query_log='ON'
```
解析
`show variables like '%slow_query_log%'`表示查看慢查询是否开启
`set global slow_query_log='ON'`表示开启慢查询日志
然后找到mysql慢查询日志路径
show variables like 'slow_query_log_file'; 这条指令可以返回慢查询日志文件的实际路径
**2.开启记录没有使用索引的慢查询**
1.查看当前状态
```sql
show variables like '%log_queries_not_using_indexes%';
```
2.开启
```sql
set global log_queries_not_using_indexes=1;
```
**3修改筛选慢查询的参数**
1.long_query_time
```sql
show global variables like '%long_query_time%'; #查找
set global long_query_time=1;
```
2.min_examined_row_limit(超过扫描了几行就会被录入慢查询日志)
```sql
show global variables like '%min_examined_row_limit%'
set global min_examined_row_limit=1;
```
#### 查看慢查询数目
```sql
show global status like '%slow_queries%';
```
这个`%slow_queries%`的结果不单单是`long_queries_time`的结果 , 也受`min_examined_row_limit`的影响
#### 测试及分析
**建表**
```sql
CREATE TABLE `student` (
`id` INT(11) NOT NULL AUTO_INCREMENT,
`stuno` INT NOT NULL ,
`name` VARCHAR(20) DEFAULT NULL,
`age` INT(3) DEFAULT NULL,
`classId` INT(11) DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```
**设置参数 log_bin_trust_function_creators**
修改这个函数是可以让存储过程函数生效.
**创建函数**
1.随机生产字符函数
```sql
DELIMITER //
CREATE FUNCTION rand_string(n INT)
	RETURNS VARCHAR(255)   #该函数会返回一个字符串
BEGIN
    DECLARE chars_str VARCHAR(100) DEFAULT
    'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
    DECLARE return_str VARCHAR(255) DEFAULT '';
    DECLARE i INT DEFAULT 0;
	WHILE i < n DO
        SET return_str =CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
        SET i = i + 1;
	END WHILE;
	RETURN return_str;
END //
DELIMITER ;

#测试
SELECT rand_string(10); #会返回一个随机10位字符
```
生产随机数值
```sql
DELIMITER //
CREATE FUNCTION rand_num (from_num INT ,to_num INT) RETURNS INT(11) #返回一个整数
BEGIN
DECLARE i INT DEFAULT 0;
SET i = FLOOR(from_num +RAND()*(to_num - from_num+1)) ;
RETURN i;
END //
DELIMITER ;

#测试：
SELECT rand_num(10,100);
```
**写存储过程**
```sql
DELIMITER //
CREATE PROCEDURE insert_stu1( START INT , max_num INT ) #注意,这里并没有返回值
BEGIN
DECLARE i INT DEFAULT 0;
    SET autocommit = 0; #设置手动提交事务
    REPEAT #循环
    SET i = i + 1; #赋值
    INSERT INTO student (stuno, NAME ,age ,classId ) VALUES
    ((START+i),rand_string(6),rand_num(10,100),rand_num(10,1000));
    UNTIL i = max_num
    END REPEAT;
    COMMIT; #提交事务
END //

DELIMITER ;
```
**调用存储过程**
```sql
#调用刚刚写好的函数, 4000000条记录,从100001号开始

CALL insert_stu1(100001,4000000);
```
**测试**
```sql
mysql> SELECT * FROM student WHERE stuno = 3455655;
+---------+---------+--------+------+---------+
|    id   |  stuno  |  name  |  age | classId |
+---------+---------+--------+------+---------+
| 3523633 | 3455655 | oQmLUr |  19  |    39   |
+---------+---------+--------+------+---------+
1 row in set (2.09 sec)

mysql> SELECT * FROM student WHERE name = 'oQmLUr';
+---------+---------+--------+------+---------+
|    id   |  stuno  |  name  |  age | classId |
+---------+---------+--------+------+---------+
| 1154002 | 1243200 | OQMlUR |  266 |   28    |
| 1405708 | 1437740 | OQMlUR |  245 |   439   |
| 1748070 | 1680092 | OQMlUR |  240 |   414   |
| 2119892 | 2051914 | oQmLUr |  17  |   32    |
| 2893154 | 2825176 | OQMlUR |  245 |   435   |
| 3523633 | 3455655 | oQmLUr |  19  |   39    |
+---------+---------+--------+------+---------+

6 rows in set (2.39 sec)
```
以上的查询都达到了秒的级别 , 说明目前的查询效率是非常的低的.

#### 慢查询日志分析工具: mysqldumpslow
这是一种最朴素的数据库监控方式 , 这种相对比开源 , 商用的监控器在单体下达到最快速 , 方便 , 损耗最小.
#### 关闭慢查询日志
一般来说 , 除了需要调优. 其它情况下 , 关闭慢查询日志是一个不错的选择
方式1: 永久性方式(在配置文件中做手脚)
```sql
slow_query_log=off #或者把这段话注释掉
```
方式2: 临时性方法
使用set语句来设置
```sql
set global slow_query_log=off
```
#### 删除慢查询日志
通过`show variables like '%slow_query_log%'`找到存放的地址位置 , 然后删除. 或者使用`mysqladmin flush-logs`重新生成查询日志文件 具体指令如下
```sql
mysqladmin -uroot -p flush-logs slow
```
慢查询日志都是使用mysqladmin flush-logs命令来删除重建的。使用时-定要注意，一旦执行了这个命令，慢 查询日志都只存在新的日志文件中，如果需要旧的查询日志，就必须事先备份。
### 查看SQL执行成本: SHOW PROFILE
show profile 在逻辑架构里提及了.
show profile 能够分析当前会话中sql都做了什么 , 执行的资源消耗情况的工具. 保存最近15次的运行结果
1一般是在会话级别开启这个功能
```sql
show variables like 'profiling'; #查看profiling状态
```
2.开启profiling功能
```sql
set profiling = 'on';
```
3.查看当前会话有哪些profilies
```sql
show profiles;
```
能够分析sql在硬件上所耗的时间
### 分析查询语句: EXPLAIN
#### 概述
定位了查询慢的sql之后 , 我们就可以使用expain或者describe工具做针对性的分析查询语句. explian和describe分析的结果是一样的
MySQL中有专门负责优化SELECT语句的优化器模块，主要功能: 通过计算分析系统中收集到的统计信息，为客户端请求的Query提供它认为最优的`执行计划`（他认为最优的数据检索方式，但不见得是DBA认为是最优的，这部分最耗费时间)。DBA 是对数据库系统进行管理和优化的专业人员(程序员)

**explain**能够让我们知道- 表的读取顺序 - 数据读取操作的操作类型。- 哪些索引可以使用 - 哪些索引被实际使用 - 表之间的引用  - 每张表有多少行被优化器查询

官方文档
[https://dev.mysql.com/doc/refman/5.7/en/explain-output.html](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html) [https://dev.mysql.com/doc/refman/8.0/en/explain-output.html](https://dev.mysql.com/doc/refman/8.0/en/explain-output.html)

**基本语法**
EXPLAIN 或 DESCRIBE语句的语法形式如下：
```sql
EXPLAIN SELECT select_options
# 或者 两个是一样的
DESCRIBE SELECT select_options
```
![](assest/Pasted%20image%2020240830171747.png)
每一列的作用
![](assest/Pasted%20image%2020240830171814.png)

**数据准备**
1.建表
表1
```sql
CREATE TABLE s1 (
	id INT AUTO_INCREMENT,
	key1 VARCHAR(100), 
	key2 INT, 
	key3 VARCHAR(100), 
	key_part1 VARCHAR(100),
	key_part2 VARCHAR(100),
	key_part3 VARCHAR(100),
	common_field VARCHAR(100),
	PRIMARY KEY (id),
	INDEX idx_key1 (key1),
	UNIQUE INDEX idx_key2 (key2),
	INDEX idx_key3 (key3),
	INDEX idx_key_part(key_part1, key_part2, key_part3)
) ENGINE=INNODB CHARSET=utf8;
```
表2
```sql
CREATE TABLE s2 (
    id INT AUTO_INCREMENT,
    key1 VARCHAR(100),
    key2 INT,
    key3 VARCHAR(100),
    key_part1 VARCHAR(100),
    key_part2 VARCHAR(100),
    key_part3 VARCHAR(100),
    common_field VARCHAR(100),
    PRIMARY KEY (id),
    INDEX idx_key1 (key1),
    UNIQUE INDEX idx_key2 (key2),
    INDEX idx_key3 (key3),
	INDEX idx_key_part(key_part1, key_part2, key_part3)
) ENGINE=INNODB CHARSET=utf8;
```
2.设置参数log_bin_trust_function_creators
这个是为了让函数一路通行
```sql
set global log_bin_trust_function_creators=1;
```
3.创建函数
```sql
DELIMITER //
CREATE FUNCTION rand_string1 ( n INT ) 
	RETURNS VARCHAR ( 255 ) #该函数会返回一个字符串
BEGIN
	DECLARE
		chars_str VARCHAR ( 100 ) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
	DECLARE
		return_str VARCHAR ( 255 ) DEFAULT '';
	DECLARE
		i INT DEFAULT 0;
	WHILE
			i < n DO
			
			SET return_str = CONCAT(
				return_str,
			SUBSTRING( chars_str, FLOOR( 1+RAND ()* 52 ), 1 ));
		
		SET i = i + 1;
		
	END WHILE;
	RETURN return_str;
	
END // 
DELIMITER ;
```
4.创建存储过程
创建往s1表中插入数据的存储过程
```sql
DELIMITER //
CREATE PROCEDURE insert_s1 (IN min_num INT (10),IN max_num INT (10))
BEGIN
    DECLARE i INT DEFAULT 0;
    SET autocommit = 0;
    REPEAT
    SET i = i + 1;
    INSERT INTO s1 VALUES(
    (min_num + i),
    rand_string1(6),
    (min_num + 30 * i + 5),
    rand_string1(6),
    rand_string1(10),
    rand_string1(5),
    rand_string1(10),
    rand_string1(10));
    UNTIL i = max_num
    END REPEAT;
    COMMIT;
END //
DELIMITER ;
```
创建往s2表中插入数据的存储过程
```sql
DELIMITER //
CREATE PROCEDURE insert_s2 (IN min_num INT ( 10 ),IN max_num INT ( 10 )) 
BEGIN
	DECLARE i INT DEFAULT 0;
	SET autocommit = 0;
	REPEAT
 	SET i = i + 1;
		INSERT INTO s2 VALUES(
				( min_num + i ),
				rand_string1 ( 6 ),
				( min_num + 30 * i + 5 ),
				rand_string1 ( 6 ),
				rand_string1 ( 10 ),
				rand_string1 ( 5 ),
				rand_string1 ( 10 ),
				rand_string1 ( 10 ));
		UNTIL i = max_num 
	END REPEAT;
	COMMIT;
	
END // 
DELIMITER ;
```
调用存储过程 , 填入相应的数据
```sql
CALL insert_s1(10001,10000); # id 10002~20001 #向表1加入数据
CALL insert_s2(10001,10000);# id 10002~20001 #向表2加入数据
```
#### EXPLAIN各列作用
##### table
表名
不论查询语句有多么的复杂 , 连接这有多少张表 , 到最后还是需要对每个表进行`单表访问`的 , explain语句输出的每条记录都对应着某个单表的访问(具有原子性的特点)
```sql
explain select count(*) from s1;
```
* 查询的每一行记录都对应一个单表
```sql
#s1:驱动表  s2:被驱动表
EXPLAIN SELECT * FROM s1 INNER JOIN s2;
# 驱动表和被驱动表是 优化器决定的，他认为哪个比较好久用哪个
```
![](assest/Pasted%20image%2020240830190800.png)
* 驱动表和被驱动表是用来描述表之间的连接顺序和执行方式的术语.
* 驱动表,是指在执行连接操作时 , 作为主要驱动的表. 优化器会首先读取驱动表中的数据 , 然后根据这些数据来查找或者连接其它表的数据 . 通常根据表的大小, 索引情况等因素来决定哪个是驱动表
  可以根据type列中all , index , range等值来帮助判断哪个表作为驱动表
* 被驱动表是驱动表的副表
* 用到多少个表 , 就有多少条记录
##### id
id是执行计划中每个查询块的标识符. 通常'id'用来区分查询中不同子查询块 , 正常来说一个select 一个 id , 也有例外的可能 , 查询优化器做了优化, 有几个select就有几个id(不一定)
```sql
explain select * from s1 where key1='a';
```
![](assest/Pasted%20image%2020240830193549.png)

```sql
explain select * from s1 inner join s2;
```
![](assest/Pasted%20image%2020240830193623.png)
* 没有复杂的连接 , 查询优化器的计划优化 , 同级别操作

```sql
explain select * from s1 where key1 in(select key2 from s2 where common_field = 'a')
```
![](assest/Pasted%20image%2020240830194339.png)
* 在一个查询语句中，每一个 `SELECT` 关键字代表一个查询块。如果多个表出现在同一个 `SELECT` 关键字下，它们的 `id` 会是相同的，因为它们属于同一个查询块。`EXPLAIN` 中的 `id` 只区分查询块，而不是表本身。查询优化器进行了优化 , 所以只有一个id , 查询优化器可能对涉及子查询的查询语句进行重写,转变为多表查询的操作 ^9139e2

```sql
explain select * from s1 where key1 in(select key1 from s2) or key2 ='a';
```
![](assest/Pasted%20image%2020240830193957.png)
* 这个有明显的复杂操作的 , 所以id会不同 , 进行了分层嘛

Union去重
```sql
#Union去重
#union 去重 , union all 不去重
explain select * from s1 union select * from s2;
```
![](assest/Pasted%20image%2020240831163356.png)
总结
1. id如果相同 , 可以认为是一组 , 从上往下顺序执行
2. 在所有组中 , id值越大 , 优先级越高 , 越先执行
3. 关注点: id号每个号码 , 表示一躺独立的查询 , 一个sql的查询趟数越少越好
##### select_type
一条大的查询语句里边可以包含若干个select关键字 , 每个select关键字代表着一个小的查询句 , 而每个select关键字的from子句中都可以包含若干张表(这些表用来做连接查询) , 每张表都对应着执行计划输出中的一条记录 , 对于在同一个select关键字中的表来说 , [他们的id是相同的](#^9139e2).
mysql为每个select关键字代表的小查询都定义了一个称之为`select_type`的属性, 意思是我们只要知道了某个小查询的`select_type`属性 , 就知道了这个`小查询在整个大查询中扮演一个什么样的角色` , 我们看一下`select_type`都能取哪些值
![](assest/Pasted%20image%2020240831160748.png)
1.simple
```sql
#查询语句中不包含`UNION`或者子查询的查询都算作是`SIMPLE`类型
explain select * from s1;
#连接查询也算是`SIMPLE`类型
explain select * from s1 inner join s2;
```
2.primary与union与union result
* union result
mysql选择使用临时表来完成union查询的去重工作 , 针对该临时表的查询的`select_type`就是`union result`, 例子上边有.
```sql
#对于包含`UNION`或者`UNION ALL`或者子查询的大查询来说，它是由几个小查询组成的，其中最左边的那个查询的`select_type`值就是`PRIMARY`
 
#对于包含`UNION`或者`UNION ALL`的大查询来说，它是由几个小查询组成的，其中除了最左边的那个小查询以外，其余的小查询的`select_type`值就是`UNION`

#`MySQL`选择使用临时表来完成`UNION`查询的去重工作，针对该临时表的查询的`select_type`就是`UNION RESULT` 	
```
测试sql:
```sql
explain select * from s1 union select * from s2;
```
![](assest/Pasted%20image%2020240831165325.png)

```sql
explain select * from s1 union all select * from s2;
```
![](assest/Pasted%20image%2020240831184924.png)

3.subquery
如果包含子查询的查询语句不能够转变为对应的semi-join的形式 , 并且该子查询是不想关子查询 , 并且查询优化器决定采用将该子查询物化的方案来执行该子查询时, 该子查询的第一个select关键字代表的那个查询的select_type就是subquery , 比如下边这个查询:
```
 #子查询：
 #如果包含子查询的查询语句不能够转为对应的`semi-join`的形式，并且该子查询是不相关子查询。
 #该子查询的第一个`SELECT`关键字代表的那个查询的`select_type`就是`SUBQUERY`
 EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2) OR key3 = 'a';
```
![](assest/Pasted%20image%2020240831185649.png)

4.dependent subquery
```sql
 #如果包含子查询的查询语句不能够转为对应的`semi-join`(表连接)的形式，并且该子查询是相关子查询，
 #则该子查询的第一个`SELECT`关键字代表的那个查询的`select_type`就是`DEPENDENT SUBQUERY`
 EXPLAIN SELECT * FROM s1 
 WHERE key1 IN (SELECT key1 FROM s2 WHERE s1.key2 = s2.key2) OR key3 = 'a';
 #注意的是，select_type为`DEPENDENT SUBQUERY`的查询可能会被执行多次。n^2
```
![](assest/Pasted%20image%2020240831185829.png)

5.dependent union
```sql
 #在包含`UNION`或者`UNION ALL`的大查询中，如果各个小查询都依赖于外层查询的话，那除了
 #最左边的那个小查询之外，其余的小查询的`select_type`的值就是`DEPENDENT UNION`。
 EXPLAIN SELECT * FROM s1 
 WHERE key1 IN (SELECT key1 FROM s2 WHERE key1 = 'a' UNION SELECT key1 FROM s1 WHERE key1 = 'b');
 
 # 这里优化器会重构成exist
```
![](assest/Pasted%20image%2020240831190728.png)

6.derived
```sql
 #对于包含`派生表`的查询，该派生表对应的子查询的`select_type`就是`DERIVED`
 EXPLAIN SELECT * 
 FROM (SELECT key1, COUNT(*) AS c FROM s1 GROUP BY key1) AS derived_s1 WHERE c > 1;
```
![](assest/Pasted%20image%2020240831191024.png)
派生表: 是通过一个子查询生成的临时表 , 该子查询出现在`from`字句中 , 并且可以被主查询使用. 派生表通常用来简化复杂查询 
派生表的特点: 1.临时表 2.用于进一步操作 3.需要起别名.
比如上面: from后面生成的就是临时表 , derived_s1就是别名

7.materialized
```sql
#当查询优化器在执行包含子查询的语句时，选择将子查询物化之后与外层查询进行连接查询时，
#该子查询对应的`select_type`属性就是`MATERIALIZED`
EXPLAIN SELECT * FROM s1 WHERE key1 IN (SELECT key1 FROM s2); #子查询被转为了物化表 
```
![](assest/Pasted%20image%2020240831193203.png)
(物化的具体含义:当数据库查询优化器决定物化子查询时，意味着它会先执行子查询，将结果集存储在临时存储空间（如临时表或内存中），而不是每次需要子查询的结果时都重新执行子查询。这种方法可以避免重复计算，提高查询效率.)
==对上面例子的解释==在没有物化的情况下，每次处理主查询（`s1`）中的每一行时，数据库都会重新执行子查询 `(SELECT key1 FROM s2)`。这样，可能会导致子查询被多次执行，尤其是在 `s1` 中有大量记录时，效率会大大降低。子查询物化处理通过物化，查询优化器可以将子查询 `(SELECT key1 FROM s2)` 的结果集一次性计算并存储在一个临时表或内存中。主查询只需要对 `s1` 中的每一行直接与这个存储的结果集进行比较，而不需要每次都重新执行子查询。
**partitions(可略)**
如果想要了解 , 查看官方文档
 https://dev.mysql.com/doc/refman/5.7/en/alter-table-partition-operations.html](https://dev.mysql.com/doc/refman/5.7/en/alter-table-partition-operations.html
##### type(重要)
执行计划的一条记录就代表着MySQL对某个表的`执行查询时的访问方法` , 又称`访问类型`, 其中的`type`列就表明了这个访问方法是啥? , 是较为重要的一个指标 . 比如 , 看到`type`列的值是`ref` , 表明MySQL即将使用`ref`访问方法来执行对`s1`表的查询

完整的访问方法如下 : system , const , eq_ref , ref , fulltext , ref_or_null , index_merge , unique_subquery, index_subquery , range , index , all

1.system
当表中`只有一条记录`并且该表使用的存储引擎的==统计数据是精确==(表示记录时提前记录了, 比如MyISAM的`count(*)`) , 比如MyISAM , Memory , 那么对该表的访问方法就是`system`.  它用于只包含一行记录的表. 是最高效的访问类型

2.const
当根据主键或者唯一二级索引列(被unique修饰过)与常数进行等值匹配时 , 对单表的访问方法就是`const`
```sql
explain select * from s1 where id = 10005;
explain select * from s1 where key2 = '10066';
```

3.eq_ref(equal reference)
在==连接查询==时 , 如果==被驱动表是通过主键或者唯一二级索引列等值匹配的方式进行访问==的(如果该主键或者唯一二级索引是联合索引的话 , 所有的索引都必须进行等值比较) , 则对该被驱动表的访问方法就是`eq_ref`
`eq_ref`是mysql查询优化器在连接查询中使用的一种非常高效的访问方法 , 如下是它的特点
* 对于每一行从驱动表中提取出来的记录 , mysql都会到被驱动表总根据主键或者唯一二级索引进行一次查找 , 并且只会找到最多一行(唯一性) , 换句话说 , `eq_ref`是mysql确保每次查询都会精确返回一个或者零个匹配的结果
```sql
explain select * from s1 inner join s2 on s1.id=s2.id; #s2.id是主键或唯一索引列
```

4.`ref`
当通过普通的二级索引与常量进行等值匹配时来查询某个表 , 那么对该表的访问方法就可能是`ref`
这个常量对应的是下面的'a'字符
```sql
explain select * from s1 where key1='a';
```
执行结果
```sql
mysql>  EXPLAIN SELECT * FROM s1 WHERE key1 = 'a';
+----+-------+------+---------------+----------+---------+
| id | table | type | possible_keys | key      | key_len |
+----+-------+------+---------------+----------+---------+
|  1 | s1    | ref  | idx_key1      | idx_key1 | 303     |
+----+-------+------+---------------+----------+---------+
1 row in set, 1 warning (0.00 sec)
```
tips: 类型相同才可以走索引
这个是不会走索引的 因为key2是字符串 , 类型不一样 , mysql会增加函数 , 进行隐式转换 , 一旦加上函数 , 就不会走索引了
```sql
explain select * from s1 where key2 = 100066;
```

5.`fulltext`
全文索引

6.ref_or_null
当对普通二级索引进行等值匹配查询 , 该索引列的值也可以是`null`值 , 那么对该表的访问方法就可能是`ref_of_null`
```sql
explain select * from s1 where key1='a' or key1 is null;
```

7.index_merge(索引合并)
单表访问方法时在某些场景下可以使用Intersection(交集) , Union(并集) , `Sort-Union`(排序并集) 这三种索引合并的方式执行查询
```sql
explain select * from s1 where key1='a' or key3 = 'a'; #这是union
```
结果如下
![](assest/Pasted%20image%2020240901171529.png)
从执行计划的type列的值是index_merge就可以看出 , mysql打算使用索引合并的方式来执行对s1表的查询

8.unique_subquery
`unique_subquery`是针对在一些包含==`in`子查询==的查询语句中 , ==如果查询优化器决定将`in`子查询转 换为`exists`[[子查询]]== , 而且子查询可以使用到主键(id是主键)进行等值匹配的话, 那么该子查询执行计划的`type`列的值是`unique_subquery`. unique_subquery是一种优化方式
[[为什么在某些场景下in子查询转变为exists子查询能够优化]]
总结来说: 1.要有in子查询 2.子查询中使用到主键 3.优化器要把in子查询转换成exists子查询
```sql
explain select * from s1 
where key2 in (select id from s2 where s1.key1 = s2.key1) or key3 = 'a';
```
上面的sql语句会转换成下面的sql语句
```sql
select * form s1 where exists(select * from s2 where s1.key2 = s1.key1 and s1.key1=s2.key1 ) or key3 = 'a';
```

9.index_subquery
优点: 使用了索引来进行查询 , **而不需要将子查询的结果集加载到内存**中. 这种类型的查询比普通的`in`子查询效率更高 , 因为它使用索引直接查找匹配的记录.
```sql
explain select * from s1 where common_field in(select key3 from s2 where s1.key1=s2.key1) or key3='a';
```

10.range
如果使用索引获取某些'范围区间'的记录 , 那么就可能使用到'range'访问方法
```sql
explain select * from s1 where key1 in ('a','b','c');
explain select * from s1 where key1 > 'a' and key1 < 'b';
```

11.index
当我们可以使用索引覆盖 , 但需要扫描全部的索引记录时 , 该表的访问方法就是`index`
```sql
explain select key_part2 from s1 where key_part3 = 'a';
```
索引覆盖 , `index idx_key_part(key+part1, key_part2, key_part3)`这三个构成一个复合索引
key_part3在复合索引里面 , 查询的字段也在索引里面 , 干脆就直接遍历索引查出数据
**possible_keys和key**
**key_len**
**ref**
**rows**
**filtered**
**Extra**
### EXPLAIN进一步使用
#### 1.EXPLAIN四种输出格式
##### 传统格式
##### JSON格式
##### TREE格式
##### 可视化输出
#### 2.SHOW WARNINGS的使用
### 分析优化器执行计划: trace
### MySQL监控分析视图-sys schema
#### Sys schema视图摘要
#### Sys schema视图使用场景
 


