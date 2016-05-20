---
title: SQL事务处理
date: 2016-04-29 19:37:42
tags: [SQL]
categories: SQL
---
##### 什么是事务？
事务，可以保证一组操作中，不会中途停止，它们要么全部执行，要么就不执行。

# 事务处理的术语
```
事务(transaction) 指一组SQL语句
回退(rollback) 指撤销指定SQL语句的过程
提交(commit) 指将未存储的SQL语句结果写入到数据库表
保存点(savepoint) 指事务处理中设置的临时占位符，可以对它发布回退。
```

##### 可以回退哪些语句？
```
事务处理用来管理INSERT、UPDATE、DELETE语句，不能回退SELECT、CREATE、DROP操作。
```