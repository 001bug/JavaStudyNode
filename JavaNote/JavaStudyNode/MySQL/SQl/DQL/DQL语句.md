**DQL语句的成分**
select 字段列表
from 表名列表
where 条件列表
group by 分组字段列表
having 分组后条件列表
order by 排序字段列表
limit 分页参数
DQL语句的执行顺序:from-->where-->group by-->select-->order by-->limit-->having


**基本查询**
1. 查询多个字段
	 select 字段1, 字段2, 字段3....from 表名;
	 select * from  表名;
2. 设置别名
	 select 字段 [AS 别名1],字段2[AS 别名2]...from 表名;
3. 去除重复记录
	 select distinct 字段列表 from 表名;

**条件查询(where)**
1. select 字段列表 from 表名 where 条件列表; 
![[../../MySQL图解.assets/Pasted image 20240302204531.png]]

**聚合函数**
1. 介绍: 将一列数据作为一个整体,进行纵向计算
2. 常见聚合函数
![[../../MySQL图解.assets/Pasted image 20240302205910.png]]
3. 语法://所有的null值是不参加聚合函数运算的 
	 select 聚合函数(字段列表) from 表名;

**分组查询(group by)**
1. 语法: select 字段列表(聚合函数在这里) from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];   
	where和having区别 
	执行时机不同: where是分组之前进行过滤, 不满足where条件, 不参与分组;而having是分组之后对结果进行过滤
	判断条件不同: where不能对聚合函数进行判断,而having可以

**排序查询(order by)**
1. 语法: select 字段列表 from表名 order by 字段1 排序方式1,字段2 排序方式2;
	 排序方式: ASC: 升序(默认值) DESC: 降序(注意: 如果是多字段排序,当第一个字段值相同时, 才会根据第二个字段进行排序)

**分页查询(limit)**
1. 语法: select 字段列表 from 表名 limit 起始索引, 查询记录数
	 起始索引从0开始,起始索引=(查询页码-1)*每页显示记录数
	 分页查询是数据库的方言,不同的数据库有不同的实现,MySQL中是LIMIT
	 如果查询的是第一页数据,起始索引可以省略,直接简写为limit 10

**表子查询**
1. 本质就是嵌套查询 
```
SELECT *
FROM emp
WHERE deptno = (
SELECT deptno
FROM emp
WHERE ename = 'SMITH'
)
```

类型主要有1单行子查询   2.多行子查询(all,any)   3.合并查询(union,union all) 无论如何都离不开SQL的查询顺序

**多表查询**
在表子查询中会过滤掉一些本该没有匹配的行,进而衍生表外链接

**分页查询**
![[../../MySQL图解.assets/Pasted image 20240711204152.png]]