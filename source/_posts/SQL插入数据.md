---
title: SQL插入数据
date: 2016-04-29 12:49:09
tags: [SQL]
categories: SQL
---
INSERT
```
INSERT INTO Customers VALUES(NULL, '张三');

# 推荐更安全、扩展性更好的方式
INSERT INTO Customers(cust_id, cust_name) VALUES(NULL, '张三');

# 进一步，无需插入 AUTO_INCREMENT属性的列
INSERT INTO Customers(cust_name) VALUES('张三');

# 插入多条数据，单条语句插入比多次语句性能更高
INSERT INTO Customers(cust_name) VALUES('张三'),('李四');

# 插入检索出来的数据
INSERT INTO Customers(cust_id,cust_name) SELECT cust_id,cust_name FROM Old_Customers;

# 从一个表复制到另外一个表
CREATE TABLE CustCopy AS SELECT * FROM Customers;
```