---
title: PHP字符串
date: 2016-05-04 10:00:24
tags: [PHP]
categories: PHP
---
###### 字符串常见的函数
chop(); 移除右端字符
trim()；移除左右两端字符
ltirm(); 移除左端字符
```
$msg = "hello world \r\n";
$name = trim($msg);
```
printf();格式化字符串
```
$total = "222";
printf("total amount of order is %s.",$total);
```
strtoupper();将字符串转换为大写；
strtolower;将字符串转为小写；
ucfirst(); 将字符串的第一个字符转换成大写；
unwords()；将字符串的每个单词的第一个字符转换为大写；
```
<?php
$total = 111;
$msg = "hello wORld";
echo strtoupper($msg)."</br>";
echo strtolower($msg)."</br>";
echo ucfirst($msg)."</br>";
echo ucwords($msg)."</br>";
?>
```
explode() 分割字符串；
strok(); 分割字符串，但一次只返回一个片段；
implode() 指定连接符连接数组中的每个元素组成字符串；
join() 指定连接符连接数组中的每个元素组成字符串;
```
<?php
$msg = "hello world!";
$array = explode(" ",$msg);
$msg3 = join("===",$array);
echo $msg3;

$msg = "hello world!";

#测试strtok();
$array = strtok($msg,"o");
echo $array;
while($array != ""){
  $array = strtok("o");
  echo "====".$array;
}
?>
```
substr(); 返回部分字符串;
```
<?php
$msg = "hello world!";
echo "count = ".strlen($msg);
echo substr($msg,0,-1)."</br>"; //得到 hello world
echo substr($msg,-5)."</br>"; //得到 orld!
echo substr($msg,0,strlen($msg)); //得到hello world!
?>
```
字符串比较
strcmp();//按字典排序，2大于12
strcasecmp();//忽略大小写，按字典排序
strnatcmp(); //按自然排序字符串，2小于12；

strlen(); //获取字符串长度
strstr();//查找字符串,返回从匹配关键字起的内容；
```
<?php
$msg = "hello world!";
echo strstr($msg,"llo"); //结果为llo world！
?>
```
str_replace(string new_str,string replacement,string info); //字符串替换
substr_replace(string string, string replace,int start);//用字符串替换指定位置
```
<?php
$msg = "hello world!";
echo substr_replace($msg,"==",-1);//结果为hello world==

$msg = "hello world!";
echo str_replace("o","===",$msg); //结果为hell===w===rld!
?>
```
##### 转义和反转义函数
addslashes($str); //将单引号和双引号等需要转义的字符，用反斜杠转义
strislashes($str);//将字符串中的转义反斜杠去掉



