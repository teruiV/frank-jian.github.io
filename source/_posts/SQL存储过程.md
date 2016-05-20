---
title: SQL存储过程
date: 2016-04-29 15:16:39
tags: [SQL]
categories: SQL
---
#### 存储过程
用户定义的一系列SQL语句的集合，设计特定表和其他对象的任务，用户可以调用存储过程。

#### 函数
通常是数据库已定义的方法，它接收参数并返回魔种类型的值并不涉及特定用户表.

#### 存储过程和函数的区别
1. 函数只能通过RETURN语句返回单个值或对象，而存储过程不允许执行RETURN，它通过OUT参数返回多个值
2. 函数可以嵌入SQL中使用，可以在SELECT中调用，而存储过程不行
3. 存储过程实现的功能复杂一些，而函数的实现功能针对性更强一些
4. 函数限制比较多，比如不能用临时表，只能用表变量，而存储过程的限制相对少一些。

#### 存储过程的优点
- 通过把处理封装在容易使用的单元中，简化复杂的操作
- 多个开发人员使用同一存储过程，保证了数据的一致性；
- 简化对变动的管理，如果业务逻辑有变化，只需要改动存储过程的代码，使用它的人员甚至都不需要知道这些变化。
- 因为存储过程通常以编译过的形式存储，而且性能更高；

#### 存储过程的缺点
- 不同DBMS的存储过程语法不同，移植性较差；
- 编写存储过程比编写简单SQL语句复杂，要求比较高；

#### 实战
```
# 创建存储过程
CREATE PROCEDURE productpricing()
BEGIN
	SELECT AVG(prod_price) AS priceaverage
    FROM products;
END;

# 调用存储过程
CALL productpricing();

# 删除存储过程

DROP PROCEDURE productpricing();

# 使用参数
CREATE PROCEDURE productpricing(
	OUT pl DECIMAL(8,2),
    OUT ph DECIMAL(8,2),
    OUT pa DECIMAL(8,2)
)
BEGIN
	SELECT Min(prod_price)
    INTO pl
    FROM products;
    SELECT Max(prod_price)
    INTO ph
    FROM products;
	SELECT AVG(prod_price)
    INTO pa
    FROM products;
END;

提示：DECIMAL(8,2)表示输出的数总共有8位，小数点有2位；

# 调用，所有MYSQL变量名都必须以@开始
CALL productpricing(@pricelow,@pricehigh,@priceaverage);

# 显示
SELECT @pricelow,@pricehigh,@priceaverage;

# IN、OUT 参数
CREATE PROCEDURE ordertotal(
	IN onumber INT,
    OUT ototal DECIMAL(8,2)
)
BEGIN
	SELECT SUM(item_price * quantity)
    FROM orderitems
    WHERE order_num = onumber;
    INTO ototal;
END;

# 调用
CALL ordertotal(20005,@ototal);

# 显示
SELECT @ototal;

提示：
IN、OUT参数相当于高级语言中的函数的传值和传引用
DECLARE定义局部变量
SHOW PROCEDURE STATUS 可以查看存储过程列表

```



参考资料：
[MySQL 存储过程和函数区别](http://fqk.io/mysql-proceduce-function-diff/)
[MySQL 笔记 5](http://fqk.io/note-mysql-5/)
