---
title: Ubuntu安装Apache+PHP+MySQL
date: 2016-05-04 10:00:54
tags: [PHP]
categories: PHP
---
我在ubuntu下配置了一个Apache服务器，通过Apache我们可以学习php网络编程，可以用它来部署自己的hexo博客。以下是安装步骤和其中踩得一些坑。
##### 步骤一，安装apache2
```
sudo apt-get install apache2
```
安装完成。
运行如下命令重启
```
sudo /etc/init.d/apache2 restart
```
出现这个问题
![](/images/apache-01.png)
ServerName没有配置好IP,编辑/var/apache2/apache2.conf如下
```
vim /var/apache2/apache2.conf
```
在apaache2.conf增加一个属性 ServerName为本机IP
![](/images/apache-02.png)
重启apache2 不会出现刚刚的问题了。

##### 步骤二，安装PHP
```
sudo apt-get install libapache2-mod-php5 php5
sudo apt-get install php5-gd php5-MySQL
```
安装完后，重新启动Apache，让它加载PHP模块
```
sudo /etc/init.d/apache2 restart
```
接下来测试Web目录下面新建一个test.php文件来测试PHP是否正常运行
```
sudo vim /var/www/html/test.php
```
然后输入
```
<?php echo '<p>Order processed.</p>'; ?>
```
接着保存文件，在浏览器里输入http://139.196.243.225/test.php,如果在网页中显示Order processed，就表示正常运行了。如果出现错误，请查看日志信息
```
 tail -f /var/log/apache2/error.log
```
##### 步骤三，安装Mysql数据库
```
sudo apt-get install mysql-server mysql-client
```
安装不成功，出现以下报错信息
```
mysql-server depends on mysql-server-5.5; however: Package mysql-server-5.5 is not configured yet. dpkg: error processing mysql-server (--configure)
```
在stackoverflow查到解决方案
```
sudo apt-get --yes autoremove --purge mysql-server-5.5
sudo apt-get --yes autoremove --purge mysql-client-5.5
sudo apt-get --yes autoremove --purge mysql-common
sudo rm -rf /var/lib/mysql /etc/mysql ~/.mysql

sudo deluser mysql
sudo apt-get autoclean
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install mysql-server-5.5 mysql-client-5.5
```
安装过程中，会要求输入root的密码，注意这里的root密码可不是ubuntu的root密码，是mysql设定的root密码，成功安装mysql。
##### 步骤四，安装phpmyadmin-mysql数据库管理
```
sudo-get install phpmyadmin
```
在安装过程中会要求选择Web server：apache2或lighttpd，使用空格键选定apache2，按tab键然后确定。然后会要求输入设置的Mysql数据库密码连接密码Password of the database’s administrative user。
然后将phpmyadmin与apache2建立连接，以我的为例：html目录在/var/www/html，phpmyadmin在/usr/share /phpmyadmin目录，所以就用命令：
```
sudo ln -s /usr/share/phpmyadmin /var/www/html
```
创建软连接。
phpmyadmin测试：在浏览器地址栏中打开http://139.196.243.225/phpmyadmin.
以上ALMP的基本组件就安装完毕了；
##### 步骤五，设置ubuntu文件执行读写权限
PHP网络服务器跟目录默认设置在：/var/www/html修改/var/www/html目录的读写权限。
```
sudo chmod 777 /var/www/html
```
然后就可以写入php文件了。
参考资料：
[ubuntu下安装Apache+PHP+Mysql](http://www.cnblogs.com/ada-zheng/p/3974963.html)
[Can't start MySQL5.5 on Ubuntu 12.04 - “dpkg: dependency problems”](http://stackoverflow.com/questions/13276088/cant-start-mysql5-5-on-ubuntu-12-04-dpkg-dependency-problems)