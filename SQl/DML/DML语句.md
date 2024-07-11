**DML简介**
DML英文全称是Data Manipulation Language(数据操作语言), 用来对数据库中表的数据记录进行增删改操作
添加数据(insert)  修改数据(update)  删除数据(delete)

**DML-添加数据**
1. 给指定字段添加数据
	 insert into 表名(字段名1,字段名2,...) values(值1,值2,...);
2. 给全部字段添加数据
	 insert into 表名 values(值1,值2,.....);
3. 批量添加数据
	 insert into 表名(字段名1,字段名2,....) values(值1,值2,...),(值1,值2,...),(值1,值2,...);
	 insert into 表名 values(值1,值2,...),(值1,值2,...),(值1,值2,...);//一般不采用这种

**DML-修改数据**
1. update 表名 set 字段名1 = 值1, 字段名2 = 值2,....[where 条件] ;//如果不加where表示所有指定的字段都会被修饰

**DML-删除数据**
1. delete from 表名 [where 条件];
注意事件: delete语句的条件可以有, 也可以没有, 如果没有条件, 则会删除整张表的所有数据     delete语句不能删除某一个字段的值(可以使用update).
