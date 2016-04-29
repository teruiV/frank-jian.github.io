---
title: SQL查询
date: 2016-04-28 17:29:35
tags: [SQL]
categories: SQL
---
#### SQL注释
```
  --    行内注释
/* */   多行注释
```

#### SELECT
```
# 返回不同的值
SELECT DISTINC name FROM person;

特别注意：不能部分使用DISTINC，如, SELECT DISTINC name, age FROM person;查询结果显示name和age都不同的记录

# 分页查询
SELECT name, age FROM person LIMIT 1,4;

# 排序检索数据
SELECT name, age FROM person ORDER BY name DESC, age DESC;

# 过滤查询
SELECT name, age FROM person WHERE age BETWEEN 23 AND 30;

## WHERE子句操作符
    <>				不等于
    !=				不等于
    <=				小于等于
    >=				大于等于
    BETWEEN AND	    在指定两值之间
    IS NULL 	    为NULL的值
    OR				匹配任一条件的记录
    IN				指定匹配清单的关键字，功能和OR相当,多个条件用逗号隔开如：IN(condition1,condition2)
    NOT			    否定其后跟的所有条件

特别注意：ORDER BY 应放于最后一条子语句，否则报错; 默认排序为升序ASC; DESC关键字只应用到直接位于其前面的列名。

# 模糊查询
SELECT name, age FROM person WHERE name LIKE '张%';

特别注意：
%  	任意0个、1个、多个字符,可以匹配任何东西，除了NULL
_	有且仅匹配一个字符


# 字段使用别名
SELECT name AS new_name,age FROM person;

# 表使用别名
SELECT cust_name,cust_contact FROM Customers AS C,Orders AS O WHERE C.cust_id = O.cust_id;

# 分组
SELECT age, name,COUNT(*) AS numbers FROM person GROUP BY age,name;

特别注意：
1、分组中如果包含具有NULL值的行，则NULL将作为一个分组返回，如果列中包含多个NULL，将它们分为同一组；
2、除聚集计算外(如：COUNT()、MAX()等)，SELECT语句中的每一列都必须在GROUP BY 子句中给出；
3、GROUP BY 子句可以包含任意数目的列

# 分组过滤
SELECT age, name, COUNT(*) AS numbers FROM person GROUP BY age,name HAVING COUNT(*) > 2;

特别注意：
HAVING等同于WHERE，唯一的区别是，HAVING可用于分组过滤，WHERE不可以。WHERE是分组前的过滤，HAVING是分组后的过滤；

# 子查询练习
查询所有购买order_item = 1的用户信息；
SELECT order_num FROM orderitems where order_item=1;

Select cust_id from orders where order_num in (SELECT order_num FROM orderitems where order_item=1);

SELECT cust_name,cust_address,cust_city FROM customer WHERE cust_id in (Select cust_id from orders where order_num in (SELECT order_num FROM orderitems where order_item=1));

# 等值联结查询
SELECT vend_name, prod_name, prod_price FROM Vendors, Products WHERE Vendors.vend_id = Products.vend_id;

特别注意：联结两表时，实际要做的是将第一个表的每一行和第二个表的每一行进行配对;如果没有联结条件，检索出行的数目将是两个表行数的乘积；

# 内联查询
SELECT vend_name, prod_name, prod_price FROM Vendors INNER JOIN Products ON Vendors.vend_id = Products.vend_id;

特别注意：内联条件用特定的ON而不是WHERE; 匹配两表都包含的记录，不包含列为NULL记录；

# 自联结查询
SELECT c1.cust_id,c1.cust_name,c1.cust_contact FROM Custormer AS c1,Custormer AS c2 WHERE c1.cust_name = c2.cust_name AND c2.cust_contact = "JIM jones";

特别注意：别忘了c1.cust_name = c2.cust_name的条件！两表联结时，检索出行的数目是两表行数的乘积；

# 外联结查询
左外联：SELECT c.cust_id,o.order_num FROM Custormers AS c LEFT OUTER JOIN Orders AS o ON c.cust_id = o.cust_id;

右外联：SELECT c.cust_id,o.order_num FROM Custormers AS c RIGHT OUTER JOIN Orders AS o ON c.cust_id = o.cust_id;

特别注意：外联包含没有关联的行，RIGHT或LEFT指定了包含其所有行的表,RIGHT指出使用右边表的所有行，LEFT则反之；且，如果包含匹配项为NULL的记录；

# 带聚合函数的联结
SELECT c.cust_id,COUNT(*) AS num FROM Customers AS c INNER JOIN Orders AS o ON c.cust_id = o.cust_id GROUP BY c.cust_id;

# 组合查询
SELECT cust_name,cust_city FROM Customers WHERE cust_name = '张三' UNION SELECT cust_name,cust_city FROM Customers WHERE cust_city = "shangHai";

等价于 SELECT cust_name,cust_city FROM Customers WHERE cust_name = '张三' OR cust_city = 'shangHai';

特别注意：
1、组合查询各个查询，查询列名应相同，且顺序也要一致；
2、UNION必须由两条或以上的SELECT语句组成
3、默认去除重复行，使用UNION ALL可以返回所有行
```
# 总结回顾
```
# SELECT子句及其顺序

SELECT		要返回的列或表达式
FROM        从中检索数据的表
WHERE	    行级过滤
GROUP BY	分组说明
HAVING		分组过滤
ORDER BY	输出排序顺序
```