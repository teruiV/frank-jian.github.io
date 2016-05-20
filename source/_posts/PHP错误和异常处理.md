---
title: PHP错误和异常处理
date: 2016-05-03 16:21:19
tags: [PHP]
categories: PHP
---
###### 异常处理的思想
当try代码中出现异常后，catch块代码对异常进行处理;
```
<?php
try{
  throw new Exception("A terrible error has occured",42);
}catch(Exception $e){
   echo " exception ".$e->getCode()." ：".$e->getMessage()." </br>"." in ".$e->getFile()." on line ".$e->getLine()."<br/>";
}
?>
```
###### Exception类内置方法
- getCode() 返回传递给构造器的代码号,不可重构
- getMessage() 返回传递给构造器的消息，不可重构
- getFile() 返回产生异常的代码文件的完整路径，不可重构
- getLine() 返回代码文件中产生异常的代码行号，不可重构
- getTrace() 返回一个包含产生异常的代码回退路径的数组，不可重构
- __toString() 允许简单地显示一个Exception对象，并给出以上所有方法可以提供的信息，可以重构

####### 自定义异常
```
<?php
class myexception extends Exception{
  function __toString(){
     return "<table border=\"1\"><tr><td><strong>Exception ".$this->getCode()."</strong>: ".$this->getMessage()."</td></tr></table></br>";
  }
}

try {
  throw new myexception("happen error,parse error",405);
}catch(myexception $e){
  echo $e;
}
?>
```