---
title: 优化SQL性能小结
date: 2016-04-28 17:04:24
tags: [SQL]
categories: SQL
---
###### 应用层面优化技巧

1、注意LIKE模糊查询的使用，避免%%，可以使用后面带% ，双%是不走索引的。
```
原来语句： select * from admin where admin_name like ‘%de%'

优化为： select * from admin where admin_name >='de' and admin_nam <'df' （注意不是等效的这里试试提供优化的思路）
```
2、尽量避免在列上进行运算，这样会导致索引失效
```
原语句： select * from admin where year(admin_time)>2014

优化为： select * from admin where admin_time> '2014-01-01′
```
3、仅列出需要查询的字段，这对速度不会有明显影响，主要考虑节省内存。
```
原来语句： select * from admin

优化为： select admin_id,admin_name,admin_password from admin
```
4、使用批量插入语句节省交互。
```
原来语句：insert into admin(admin_name,admin_password) values (‘test1′,'pass1′);

insert into admin(admin_name,admin_password) values (‘test2′,'pass2′);

insert into admin(admin_name,admin_password) values (‘test3′,'pass3′)

优化为： insert into admin(admin_name,admin_password) values(‘test1′,'pass1′),(‘test2′,'pass2′),(‘test3′,'pass3′)
```
5、limit的基数比较大时使用between。
```
原来语句：select * from admin order by admin_id limit 100000,10

优化为：  select * from admin where admin_id between 100000 admin 100010 order by admin_id
```
6、不要使用rand函数获取多条随机记录。
```
原来语句： select * from admin order by rand() limit 20

优化为： select * from admin as t1 Join(select round(rand()*((select max(admin_id) from admin)-(select min(id) from admin))+(select min(id) from admin)) as id) as t2 where t1.id>=t2.id order by t1.id limit
```
7、避免使用NULL
8、不要做无谓的排序操作，而应尽可能在索引中完成排序
9、不要使用count(col)，而应该是count(*)


###### count(col)和count(*)有什么区别？
count(*)通常是对主键进行索引扫描，统计表中所有符合的记录总数;而count(col)是对某个字段进行扫描，统计表中所有符合COL的记录总数;顺便提一下，count(col)统计记录总数时，是不包含col值为NULL的记录的；

###### count时的WHERE执行原理？
count的时候，如果有WHERE限制的情况，总是需要对MYSQL进行全表遍历，然后返回所得记录的总数；
