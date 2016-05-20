---
title: MySQL配置文件设置my.ini
date: 2016-05-04 09:22:12
tags: [MYSQL]
categories: MYSQL
---
//客户端设置
[client]

//设置mysql客户端连接服务器时默认使用端口
port = 3306

[mysql]

//设置mysql客户端默认字符集
default-character-set=utf8

//服务器端设置
[mysqld]

//mysql服务器默认监听(listen on)的TCP/IP端口
port=3306

//基准路径
basedir="D:/software/MYSQL5.6"

//mysql数据库文件所在目录
datadir="D:/software/MYSQL5.6/Data"

//服务端使用的字符集默认为latin1字符集，设置为utf8
character-set-server=utf8

//默认存储引擎为INNODB
default-storage-engine=INNODB
