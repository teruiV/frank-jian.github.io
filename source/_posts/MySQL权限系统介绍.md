---
title: MySQL权限系统介绍
date: 2016-05-04 09:57:29
tags: [MYSQL]
categories: MYSQL
---
##### 创建用户
```
//创建用户流云
CREATE USER  "liuyun"@'localhost' IDENTIFIED BY '123456';
```
##### 授权
```
注：privileges -用户操作权限如SELECT、INSERT、UPDATE、DELETE，如果要授予所有权限则使用ALL
GRANT privileges ON databasename.tablename TO 'username'@'host'；

# 对 test数据库的所有表具有增删改查的权限；
GRANT SELECT,INSERT,UPDATE,DELETE  ON test.* TO 'liuyun' @ 'localhost';
FLUSH PRIVILEGES;
```
#### 切换用户
```
exit;
mysql -u liuyun -p liuyun
```
#### 更改用户密码
```
#更改其他用户
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');

#当前用户修改密码
SET PASSWORD = PASSWORD('newpassword');
```
#### 撤销用户权限
```
REVOKE SELECT,INSERT,UPDATE,DELETE ON test.*  FROM  'liuyun'@'localhost';
```
##### 查看某个用户有哪些权限
```
SHOW GRANTS FOR 'liuyun'@'localhost';
```
##### 删除用户
```
DROP USER "liuyun" @ "localhost";
```


