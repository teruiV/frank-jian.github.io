---
title: SQL创建表
date: 2016-04-29 14:40:38
tags: [SQL]
categories: SQL
---
##### 创建表
```
# 创建表
CREATE TABLE Products(
 prod_id	CHAR(10)	NOT NULL,
 vend_id	CHAR(10) 	NOT NULL,
 prod_name	CHAR(254)	DEFAULT 'xxx',
 PRIMARY KEY  (prod_id)
)ENGINE=InnoDB;

# 添加列
ALTER TABLE Products ADD prod_price INT;

# 删除列
ALTER TABLE Products DROP COLUMN prod_price;

特别注意：更新/删除表之前最好做一个完整的备份，数据库的更改不能撤销;

#定义外键
ALTER TABLE Products
ADD CONSTRAINT vend_id
FOREIGN KEY(vend_id) REFERENCES Vendor(vend_id);

#定义外键的好处
两张表关联,保证数据的一致性

#重命名表
RENAME TABLE Customers TO customers;
```
##### 主键
唯一标识表示表中的每一行的列；
##### NOT NULL
指定列名不为空；
##### DEFAULT
插入时，如果不给出值，自动采用默认值,MYSQL中默认值只支持常量，不允许使用函数
##### 引擎类型
- InnoDB是一个可靠的事务处理引擎，它不支持全文搜索
- MEMORY在功能等同于MYISAM，但由于数据存储在内存中，速度很快(适合创建临时表)
- MyISAM是一个性能极高的引擎，它支持全文搜索，但不支持事务处理
- 引擎可以混用，但外键不能跨引擎


