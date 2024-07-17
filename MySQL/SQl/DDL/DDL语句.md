DDL全称[Data Definition Language]  (数据定义语言)
**DDL-数据库操作**
1. 查询 //必须要有分号 
	1.查询所有[数据库]: show databases ;
	2.查询当前数据库: seclect database();//一般在已经执行了很多条语句的时
2. 创建
	create database [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET字符集]   [COLLATE 排序规则]  ;//[]起来的可选
3. 删除
	drop database[IF EXISTS] 数据库名;
4. 使用
	use 数据库名



**DDL-表操作-查询**
1. 查询当前数据库所有表  show tables;
2. 查询表结构  desc 表名; 
3. 查询指定表的建表语句  show create table 表名;//显示表的语法嘛
**DDL-表操作-创建**
```
CREATE TABLE 表名{
	字段1 字段1类型[comment 字段1注释], 
	字段2 字段2类型[comment 字段2注释],
	字段3 字段3类型[comment 字段3注释]
}[comment 表注释];
```
注意: [ ]内的内容可选, 最后一个无逗号, 一个分号表示一整条语句

**DDL-表操作-修改**
1. 添加字段
	 alter table 表名 add 字段名 类型(长度) [comment注释]   [约束];
2. 修改数据类型
	 alter table 表名 modify 字段名 新数据类型(长度);
3. 修改字段名和字段类型
	 alter table 表名 change 旧字段名 新字段名 类型(长度) [commet 注释]   [约束];
4. 删除字段
	 alter table 表名 drop 字段名;
5. 修改表名
	 alter table 表名 rename to 新表名;
6. 删除表
	 drop table[if exists] 表名;
7. 删除指定表, 并重新创建该表
	 truncate table 表名;