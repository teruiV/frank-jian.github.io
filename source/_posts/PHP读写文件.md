---
title: PHP读写文件
date: 2016-05-04 09:59:20
tags: [PHP]
categories: PHP
---
##### 写文件步骤
1. 打开文件，如果文件不存在，需要先创建它；
2. 将数据写入这个文件；
3. 关闭这个文件；

##### 读文件步骤
1. 打开这个文件，如果文件不能打开，说明文件不存在就正确地退出;
2. 从文件中读出数据
3. 关闭文件;

##### 打开文件
```
# $_SERVER['DOCUMENT_ROOT'] 表示当前文件的路径；
$fp = fopen( "$_SERVER['DOCUMENT_ROOT']/../orders/orders.txt ", 'w' ); 

//如果文件不能正确地打开则退出；
if($fp) {
	echo "<p>Your Order could not be processed at this time.Please try again later.</p> ";
    exit;
}
```

##### 写文件
```
$fp = fopen( "$_SERVER['DOCUMENT_ROOT']/../orders/orders.txt ", 'w' ); 
$outputstring = "msg";

# 使用二进制模式执行写操作，第三个参数可以避免一些跨平台的兼容问题；
fwrite($fp,$outputstring,strlen($outputstring)); //写到字符串末尾或已经输入length字节就停止写入。
```
##### 关闭文件
```
$fp = fopen( "$_SERVER['DOCUMENT_ROOT']/../orders/orders.txt ", 'w' );
fclose($fp);  //正确关闭文件返回true，否则返回false;
```
##### 读文件
```
$fp = fopen( "$_SERVER['DOCUMENT_ROOT']/../orders/orders.txt ", 'rb' );
#判断文件何时读完
while(!feof($fp)){
	$order = fgets($fp,999); //从文件中每次读取一行的内容；
}
```
##### 单行读取方式
```
$fp = fopen( "$_SERVER['DOCUMENT_ROOT']/../orders/orders.txt ", 'rb' );

1、fgets($fp,999); //从文件中每次读取一行内容；

2、fgetss($fp,999,tags); //每次读取一行内容，并且过滤字符串中包含的PHP或HTML特殊标记; tags为字符串数组表示，需要过滤的一些标记,如，html、body

3、$order = fgetcsv($fp,100,"\t")；//从文件从读取一行，并且在有制表符（\t）的地方将文件内容分行。

4、fread($fp,length),从文件中读取length字节内容；
```
##### 读取整个文件 （经过测试，发现读取整个文件的方法均不能实现，为何？）
```
$filename =$_SERVER['DOCUMENT_ROOT']/orders/order.txt;
1、readfile（$filename）; //读取整个文件：
2、file($filename);//读取整个文件，将结果发送到一个数组，每一行为一个元素；
```
##### 文件的定位
```
$fp = fopen( "$_SERVER['DOCUMENT_ROOT']/../orders/orders.txt ", 'rb' );
1、rewind($fp); //将文件制定复位到文件的开始；
2、ftell($fp); //报告文件指针在当前文件中的位置；
3、fseek($fp,offset,current),//文件指针从current位置，移动offset个字节；
```
##### 文件锁
**使用文件锁的目的？**
假设两个用户试图同时订购同一件商品，那么生成的订单是第一个用户的还是第二个用户的；为了避免这样的情况，可以使用文件锁；
```
$fp = fopen( "$_SERVER['DOCUMENT_ROOT']/../orders/orders.txt ", 'ab' );
flock($fp,LOCK_EX)；//锁定文件的写操作；
$outputstring = "msg";
fwrite($fp,$outputstring);
flock($fp,LOCK_UN);//释放锁；
fclose($fp);
```
##### flock操作值
```
LOCK_SH 读操作锁定，文件可以共享，其他人可以读该文件；
LOCK_EX 写操作，互斥，文件不能被共享
LOCK_UN 释放已有的锁定
LOCK_NB 防止在请求加锁时发生阻塞
```
##### 实战
```
<?php
$DOCUMENT_ROOT=$_SERVER['DOCUMENT_ROOT'];
$fp = fopen("$DOCUMENT_ROOT/../orders/orders.txt",'ab');
flock($fp,LOCK_EX);
fwrite($fp,"write the file twice");
flock($fp,LOCK_UN);
$fp = fopen("$DOCUMENT_ROOT/../orders/orders.txt",'rb');
while(!feof($fp)){
   $content = fgets($fp,999);
   echo "content = ".$content;
}
fclose($fp);
?>
```
