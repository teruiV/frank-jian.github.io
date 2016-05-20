---
title: PHP面向对象特性
date: 2016-05-05 20:04:49
tags: [PHP]
categories: PHP
---
常量声明
```
<?php
//特别注意 PI没有$,同时不用声明访问权限，声明为public时，会报错；
class Fruit{
  const PI =3.14;
}
echo Fruit::PI;
?>
```
define和const的区别？
1. 编译器处理方式不同，define宏是预处理阶段展开，const常量是编译运行阶段使用;
2. 类型和安全检查不同；define宏没有类型，不做任何类型检查，仅仅是展开；const常量有具体的类型，编译阶段会执行类型检查；
3. 存储方式不同，define宏仅仅是展开，有多少地方使用，就展开多少次，不分配内存，宏定义不分配内存，定义变量分配内存；const常量会在内存中分配；
4. const可以节省空间，避免不必要的内存分配；

静态类成员和方法的调用
```
<?php
class Fruit{
public static $count = 0;

public static function add(){
   self::$count++;
}

public static function get(){
  return self::$count;
}
}
//注意调用静态方法不需要用$符号；
Fruit::add();
echo Fruit::get();
?>
```
getter和setter方法;
```
<?php
class Fruit {
  private $name;
  private $price;
 
  //为private访问权限也能访问，因为拦截于类内
  public function __set($_key,$_value){
        $this->$_key = $_value;
  }

  public function __get($_key){
    return $this->$_key;
  }
}
$fruit = new Fruit();
$fruit->name = "apple";
$fruit->price = 2.0;
echo $fruit->name. " -> ". $fruit->price;
?>
```
多态
```
<?php
interface Computer{
   public function name();
   public function version();
}

class Tcomputer implements Computer{
  public function name(){
   echo "apple";
  }

  public function version(){
     echo "windowxp";
  }
}

class Person{
 public function run($type){
     $type->name();

     $type->version();
  }
}

$person = new Person();
$computer = new Tcomputer();

$person->run($computer);

?>
```
自动引入多个类文件
```
<?php
        //引入类文件即可
        //require 'computer.class.php';
        //1.如果要包含多个类文件，是不是要一一引入。
        //2.引入多个类文件，如果有用不到的，就会产生浪费
        //3.引入了类文件，可能会遗漏，比如说，创建了一个对象，而这个对象的类文件没有引入。就会产生错误

        //$_className = 类名
        //只要实例化了，那么 $_className = 'Computer';

        function __autoload($_className) {
                require strtolower($_className).'.class.php';
        }


        $computer = new Computer();
        $person = new Person();
        echo $computer->_name;
        $computer->_run();
?>
```
__toString
```
<?php
// 设置__toString()的访问权限为private,仍然能够访问__toString()访问；类内拦截访问该方法；
        class Computer {
                public function _run() {
                        echo 'run';
                }
                private function __toString() {
                        return 'i am string';
                }
        }
        echo new Computer();
?>

```