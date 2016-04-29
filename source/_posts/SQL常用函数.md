---
title: SQL常用函数
date: 2016-04-28 20:42:48
tags: [SQL]
categories: SQL
---
#### 常用文本处理函数
```
Concat(str1,str2,str3)拼接字符串
Ltrim()	去掉左边的空格
Trim()	去掉所有的空格
Left()	返回串左边的字符
Locate() 找出串的一个子串
Lower() 将串转换为小写
Right() 返回串右边的字符
Soundex() 返回串的SOUNDEX值
SubString() 返回子串的字符
Upper() 将串转换为大写
```
#### 日期和时间处理函数
```
AddDate() 增加一个日期（天、周等）
AddTime() 增加一个时间（时、分等）
CurDate() 返回当前日期
CurTime() 返回当前时间
Date()
DateDiff()
Date_Add()
Date_Format()
Day()
DayOfWeek()
Hour()
Minute()
Month()
Now()
Second()
Time()
Year()

# 实战
SELECT cust_id, order_num FROM orders WHERE Date(order_date) BETWEEN '2005-09-01' AND '2005-09-30';

SELECT cust_id, order_num FROM orders WHERE Year(order_date) = 2005 AND Month(order_date) = 9;
```
#### 数值处理函数
```
Abs() 绝对值
Cos()
Exp() 指数
Mod()
Pi() 
Rand()
Sin()
Sqrt() 平方根
Tan() 正切值
```
