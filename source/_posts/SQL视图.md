---
title: SQL视图
date: 2016-04-29 14:47:16
tags: [SQL]
categories: SQL
---
#### 什么是视图？
视图是虚拟的表，与包含数据的表不一样，视图只包含使用时动态检索数据的查询
#### 使用视图的好处？
- 保护数据，可以授予用户访问表的特定部分的权限，而不是整个表的权限；
- 更改数据格式和表示，视图可返回与底层数据表的表示和格式不同的数据；
- 重用SQL；
- 简化复杂的SQL

#### 视图的规则
1. 视图需唯一命名
2. 创建视图，必须具有足够的权限
3. 视图不能索引，也不能关联触发器或默认值
4. 视图可以和表一起使用
5. 创建视图的数目没有限制
6. ORDER BY 可以用在视图中，若SELECT语句中也有ORDER BY，则会覆盖视图中的ORDER BY
7. 视图可以嵌套

#### 适用场景
视图一般只用于检索，而不用于更新

#### 实战
```
# 创建视图
CREATE VIEW ProductCustomers AS
SELECT cust_name,cust_contact,prod_id
FROM Customers,Orders,OrderItems
WHERE Customers.cust_id = Orders.cust_id
AND OrderItems.order_num = Orders.order_num ;

# 查询视图数据
SELECT * FROM ProductCustomers;

# 移除视图
DROP VIEW ProductCustomers;
```

