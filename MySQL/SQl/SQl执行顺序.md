面试题
`计算总和这部分的sql语句`  
`SELECT order_num, Sum(item_price * quantity) AS total_price`  
`FROM OrderItems`  
`GROUP BY order_num`  
`HAVING total_price >= 1000`  
`ORDER BY order_num`

1.FROM子句确定基本的数据来源,先执行,然后执行join 和 on,进行表外链接这中间,底层会产生临时表
2.WHERE子句对从FROM字句获得的数据进行行级过滤,过滤条件可以多个条件语句联合筛选,在where子句中不可以使用聚合函数,不能使用别名
3.GROUP BY 字句对数据进行分组,分组的约束,在select语句中要有,并且,select中都是聚合函数
4.HAVING字句基于每个组进行条件过滤,此时是可以使用SELECT列表中定义的别名的，因为在这个阶段，SQL引擎已经知道了每个组的聚合值(SQL引擎会先计算)。版本更新的变化
5.SELECT字句选择并计算每个组的聚合值或其他列,比如Sum(item_price * quantity) AS total_price`
6.distinct对select产生的结果进行去重操作
7.ORDER BY 子句对结果进行排序(这个过程是最耗资源的)
8.LIMIT
