DCL-介绍
DCL全称是Data Control Language(数据控制语句), 用来管理数据库, 用户,控制数据库的访问权限.

**DCL-管理用户**
1. 查询用户
use mysql;
select * from user;
2. 创建用户
create user '用户名'@'主机名' identified by '密码';
3. 修改用户密码
alter user '用户名'@'主机' identified with mysql_native_password by '新密码';
4. 删除用户名
drop user '用户名'@'主机名';

**DCL-权限控制**
MySQL中常见的权限
![[Pasted image 20240306130847.png]]
1. 查询权限
show grantis for '用户名'@'主机名';
2. 授予权限
grant 权限列表 on 数据库名.表名 to '用户'@'主机名';
3. 撤销权限
revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
