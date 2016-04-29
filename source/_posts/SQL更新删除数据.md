---
title: SQL更新删除数据
date: 2016-04-29 13:43:53
tags: [SQL]
categories: SQL
---
UPDATE
```
# 更新一条数据
UPDATE Customers SET cust_email = "kim@163.com" WHERE cust_id = "10005";

特别注意：别忘了WHERE条件，否则将影响整张表;

```
DELETE
```
# 删除一条记录
DELETE FROM Customers WHERE cust_id = "10006";

特别注意：别忘了WHERE条件，否则将删除整张表,推荐先用SELECT查询需要删除的记录，再执行删除操作;

#删除整个表
DROP TABLE Customers;

# 外键的好处
使用外键确保完整性的好处是，可以防止删除某个关系需要用到的行。
```