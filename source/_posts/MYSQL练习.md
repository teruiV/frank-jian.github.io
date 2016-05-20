---
title: MYSQL练习
date: 2016-05-04 09:53:55
tags: [MYSQL]
categories: MYSQL
---
#### 第一题
> 表student:stu_id、stu_name、stu_age、company   -- 主键 stu_id
> 表class:class_id、class_name  -- 主键 class_id
> 表result: stu_id、class_id、score -- 组合主键 stu_id、class_id  外键 stu_id、class_id

1、使用标准SQL嵌套语句查询选修课程名称为'math’的学员学号和姓名?
2、使用标准SQL嵌套语句查询选修课程编号为‘c_3’的学员姓名和所属单位?
3、使用标准SQL嵌套语句查询不选修课程编号为’c_3’的学员姓名和所属单位?
4、查询选修了课程的学员人数
5、查询选修课程超过2门的学员学号和所属单位?
6、查询每个学生所选的课程数，显示学号、学生名字、和所选课程的数目；

1、
result:
方法1：子查询
select class_id from class where class_name='math';
select student_id from result where class_id  in ()
select stu_id,stu_name from student where stu_id in ();
==>
select stu_id,stu_name from student where stu_id in (select stu_id from result where class_id  in (select class_id from class where class_name='math'));

方法2：等值联结
select student.stu_id,stu_name from student,class,result where class_name='math' AND class.class_id = result.class_id AND result.stu_id = student.stu_id;

2、
方法1：子查询
select stu_id from result where class_id = 'c_3';
select stu_name,company from student where stu_id in ();
==>
select stu_name,company from student where stu_id in (select stu_id from result where class_id = 'c_3');
方法2：内联结
SELECT stu_name,company FROM student INNER JOIN result ON class_id = 'c_3' AND result.stu_id = student.stu_id;

3、
select stu_id from result where class_id = 'c_3';
select stu_name,company from student where stu_id NOT in();
==>
select stu_name,company from student where stu_id NOT in(select stu_id from result where class_id = 'c_3');

4、
select  COUNT(DISTINCT stu_id) AS count from result;

5、
select stu_id from result group by stu_id having count(*) >=2;
select stu_id ,company from student where stu_id in ();
==>
select stu_id ,company from student where stu_id in (select stu_id from result group by stu_id having count(*) >=2);

6、
create view course_count as select stu_id,count(*) as num from result group by stu_id;

select student.stu_id,stu_name,course_count.num from student,course_count where student.stu_id=course_count.stu_id;

#### 第二题
查询A(ID,Name)表中第31至40条记录，ID作为主键可能是不是连续增长的列
SELECT * FROM A LIMIT 30,10；
Tips: 分页查询的下标索引是从0开始。

#### 第三题
查询表B中存在ID重复三次以上的记录
select id,count(*) as num from B group by id having  num > 3;
#### 第四题
> 表 customers:   customerid、name、city、address
> 表 orders :   customerid、orderid、amount、date

######  每个用户买个几本书，注意是没有买过书的用户显示数量为0；
select customers,count(orders.orderid) from customers left join orderid on customers.customerid = order.customerid group by customerid;
######  检索出消费超过100元的vip用户;
select customerid,sum(orders.amount)  as sum from customers left join orders on customers.customerid = orders.customerid group by customerid  having sum >= 100;
######  统计每个用户每个订单消费超过50的总和
select customerid,ifnull(sum(orders.amount),0)  as sum from customers left join orders on customers.customerid = orders.customerid and orders.amount >= 50 group by customerid ;
######  统计这个月每个用户的消费金额
select customerid,ifnull(sum(orders.amount),0)  as sum from customers left join orders on customers.customerid = orders.customerid and  date between date("2016-05-01") and date("2016-05-31") group by customerid ;
######  统计每个用户的消费金额
select customerid,ifnull(sum(orders.amount),0)  as sum from customers left join orders on customers.customerid = orders.customerid group by customerid ;


##### ISNULL IFNULL、NULLIF函数

1、isnull(expr)  expr为null，为1，不为空为0；
```
SELECT ISNULL(null)     # 结果为 1
SELECT ISNULL(1) #结果为0
```
2、ifnull(expr1,expr2)，expr1为空,值等于expr2,不为空则为expr1
```
SELECT userid,ifnull(item.itemid,0) from user left join item on item.userid = user.userid ; #结果为当用户没有下单订时，该用户检索出的订单将默认为0，而不是空；
```

3、nullif(expr1,expr2), expr1不等于expr2，值为expr1，否则为空；
```
SELECT NULLIF(201,111)    #结果为 201，因为201！=111,所以结果为201，否则为null；
```

##### WHERE 、HAVING、ON的区别是什么？
ON，是表联结的条件；
WHERE，定义表行记录的过滤条件,
HAVING，定义分组的过滤条件；

##### WHERE和HAVING的区别
HAVING子句可以引用总计函数，而WHERE子句不能引用；
HAVING必须引用GROUP BY子句中的列或总计函数的列；
HVING位于GROUP BY 和ORDER BY之间；




