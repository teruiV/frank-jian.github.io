---
title: PHP语法基础
date: 2016-05-04 10:00:03
tags: [PHP]
categories: PHP
---
##### PHP数据类型
- Integer  -- 整数
- Float  -- 浮点数，也叫Double，双精度值，用来表示实数；
- String -- 字符串，
- Boolean -- 布尔值，表示true或者false
- Array -- 数组，用来保存有相同类型的多个数据项
- Object -- 对象，用来保存类的实例
- NULL -- 没有被赋值，已经被重置或者被赋值为特殊值NULL的变量
- resource -- 特定的内置函数（例如数据库函数）将返回resource类型变量，代表外部资源（如数据库连接）

##### 声明常量
define('PRICE ', 10);
```
echo PRICE;
```
##### 字符串连接符
一个点号 (.)，可以将极端文本连接成一个字符串。
`echo $.msg. 'try do it <br/>' ;`

##### 单引号和双引号的区别？
单引号中，变量名称或者其他任何文本都会不经修改，发送给浏览器；
双引号中，变量名称会被解析，然后发给浏览器；
```
//假设$msg = "info"
echo "msg = $msg"  #结果为 msg = info;
echo 'msg = $msg'  #结果为 msg = $msg;
```
##### 引用操作符
引用操作符&可以在关联赋值中使用，通常，在一个变量赋值给另外一个变量的时候，先产生原变量的一个副本，然后再将它保存在内存的其他地方，使用操作符&可以避免产生这样的副本。
```
$a = 5;
$b = $a;
$a = 7;
echo "a = $a, b = $b"; //结果为a = 5, b = 7;

$a = 5;
$b = &$a;
$a = 7;
echo "a = $a, b = $b"; //结果为a = 7, b = 7;
```
##### 错误抑制操作符
错误抑制操作符@可以在任何表达式面前使用，可以抑制一些警告，但要写一些错误处理的代码；
```
$a = @(57/0);
```
如果在php配置文件中的track_errors特性，错误信息将会被保存在$php_errormsg中。

##### 执行操作符
一对反向单引号(`  `  )，将反向单引号之间的命令当做服务器端的命令来执行；
```
$out = `ls -la`  ;
echo '<pre>' .$out. '</pre>';
```
##### 类型操作符
类型操作符instanceof，用来检查一个对象是否是特定类的实例；
```
class sampleClass{};
$myObject = new sampleClass();
if ($myObject instanceof sampleClass) {
  echo “myObject is an instance of sampleClass”；
}
```
##### 测试变量状态
bool isset(mixed var);如果变量存在，则返回true，如果不存在则返回false；
void unset(mixed var);销毁变量；
bool empty(mixed var);检查变量，为null,为空字符串，为0，均返回true，其余返回false；
```
$msg = "msg";
if(isset($msg))
  echo "isset function return true";
else
  echo "isset function return false";
unset($msg);
if(isset($msg))
  echo "isset function return true";
else
  echo "isset function return false";
}
```
##### 访问表单数据的几种方式：
- $name    //简短风格，需要将register_globals配置设为on，默认是off，有安全问题；
- $_POST["name"] ,$_GET["name"]  //推荐使用，没有安全问题
- $HTTP_POST_VARS["name"]  //冗长风格，性能不好；

```
//hello.php
<form action="hello.php" method="post">
      enter your name:<input type="text" name="name">
      enter your age:<input type="text" name="age">
     <input type="submit" value="submit">
</form>
Welcome <?php echo$_POST["name"];?>  <br/>
You are <?php echo $_POST["age"];?> years old!
```
#####  当函数返回值为false时，需要用“===”来判断
因为布尔值“true”和"false"可以分别用整数“1”和“0”来表示；
```
<?php
function large($x,$y){
  if((!isset($x)) || (!isset($y))){
      return false;
  } else if ($x > $y){
      return $x;
  } else {
     return $y;
  }
}
$x = 0;
$y = -1 ;
$result = large($x,$y);
echo "return = $result <br/>" ;
#因为布尔值false可以用0来表示，所以为了确保0和false不混淆，用"==="
if($result === "false"){
  echo "false";
} else {
  echo $result;
}
?>

```




