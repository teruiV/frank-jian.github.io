---
title: SQL视图
date: 2016-04-29 14:47:16
tags: [SQL]
categories: SQL
---
#### 什么是视图？
视图是虚拟的表，只包含使用时动态检索的查询，同时视图本身是不包含数据的，返回的数据是从其他表检索出来的。
#### 使用视图的好处？
- 保护数据，只让开发者看到表中的部分数据；
- 更改数据格式和表示，视图可返回与底层数据表的表示和格式不同的数据；
- 重用SQL；
- 简化复杂的SQL,在编写查询后，可以方便的重写它而不必知道其他查询细节
- 可以嵌套查询，支持子查询；

#### 视图的规则
1. 视图需唯一命名
2. 创建视图，必须具有足够的权限
3. 视图不能索引，也不能关联触发器或默认值
4. 视图可以和表一起使用
5. 创建视图的数目没有限制
6. ORDER BY 可以用在视图中，若SELECT语句中也有ORDER BY，则会覆盖视图中的ORDER BY
7. 视图可以嵌套
8. 视图只做只读查询，不能将数据写回底层表；

#### 视图的缺点
因为视图不包含数据，所以每次使用视图是，都必须处理查询执行时的所有检索。如果使用多个联结和过滤创建了复杂的视图或者嵌套视图，性能可能下降的很厉害。

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

