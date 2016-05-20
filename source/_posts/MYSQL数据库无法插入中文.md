---
title: MYSQL数据库无法插入中文
date: 2016-04-29 12:31:48
tags: [SQL]
categories: SQL
---
##### MYSQL无法插入中文，报Incorrect string value: '\xD5\xC5\xC8\xFD'错误
`mysql> status;`
![](/images/mysql中文无法插入.png)
##### 使用的库的字符集是latin1，该字符集不支持中文字符
`mysql> show CREATE TABLE person1;`
![](/images/mysql中文无法插入2.png)
##### 修改表字符集
`mysql> ALTER TABLE person1 character set utf8;`

`mysql> show CREATE TABLE person1;`
![](/images/mysql中文无法插入3.png)

##### 修改字段字符集
`mysql> ALTER TABLE person1 modify name varchar(20) character set utf8;`
![](/images/mysql中文无法插入4.png)

##### 这时在windows下的CMD控制台的MYSQL中执行
`mysql>INSERT INTO person1(id,name,age) VALUES (7,'张三',21);`
![](/images/mysql中文无法插入5.png)
##### 发现仍然报错，什么原因呢？
![](/images/mysql中文无法插入6.png)
因为CMD控制台的编码是GBK的，但MYSQL的编码是UTF8所以出错;最后，用第三方软件navicate，在person1表中，执行刚刚的插入操作是可以成功.
##### 最后设置数据库的编码：
`mysql>show variables like 'character_set_%';`
![](/images/mysql中文无法插入7.png)
`mysql>set character_set_database=utf8;`
![](/images/mysql中文无法插入8.png)
##### 关于MYSQL数据库的配置
```
1、修改mysql/my.ini 配置文件
[client]
default-character-set=utf8

[mysqld]
default-storage-engine=INNODB
character-set-server=utf8

2、重启MYSQL
#windows下，停止MYSQL服务
net stop mysql

#启动服务
net start mysql

3、修改成功，进入MYSQL查看字符集
show variables like 'character_set_%';
```
