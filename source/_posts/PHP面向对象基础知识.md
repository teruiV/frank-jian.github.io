---
title: PHP面向对象基础知识
date: 2016-05-03 09:33:00
tags: [PHP]
categories: PHP
---
##### 类
对象直接彼此互不相同，但具有一些共同点的对象集合
##### 对象
举个例子，我的自行车可以被认为自行车类的一个对象，他拥有所有自行车的共同特征；
##### 多态性
不同的类对同一个操作可以有不同的行为；
##### 继承
在已有类的基础上创建新类，新类包含已有类的所有的方法和属性且可以添加新的方法；PHP中只支持单重继承类，不支持多重继承类；要实现多次继承可以通过接口；

##### 构造函数
初始化对象和属性,其名称必须是 __constructor().
```
class classname
	function __construct($param){
       echo "Constructor called with parameter".$param."</br>";
    }

```
##### 析构函数
在销毁一个类之前执行的操作，在类的引用被重置或者超出作用域时**自动发生**。
##### 访问权限
- public ,设置为public的属性和方法，可以在类的内部和外部使用；
- private,设置为private的属性和方法，只能在类的内部进行访问,方法和属性将不会被继承；
- protected,设置为protected的属性和方法，在类的子类和类的内部中可以进行访问；
注：未指明方法默认为public权限，属性一定要声明访问权限否则会报错;

##### final关键字
- 在函数前加final，这个函数将不能被在子类中被重载；
- 在类前加final，这个类将不能被继承;

##### Per-class常量
使用操作符::指定常量所属的类来访问Per-class变量;
```
<?php
  class Math{
  	const pi = 3.14;
  }
  echo "Math :: pi = ".Math::pi."\n";
?>
```
##### 静态方法
在方法前添加static关键字
```
class Math{
   static function squared($input){
   	return $input *$input;
   }
}
echo Math::squared(8);
```
##### 检查类的类型和类型提示
instanceof可以检查一个对象是否是特定类的实例;是否实现了某个接口；
```
<?php
class A{
 public function method1(){
   echo "method1";
 }
}
interface C {
  public function method3();
}

class B extends A implements C{
 function method2(){
    echo "method2";
 }
 function method3(){
     echo "method3";
  }
}

$b = new B();
$a = new A();

if($b instanceof A){
   echo "\$a is an object of B <br/>";

}else{
   echo "\$a is not an object of B<br/>";
}

if($b instanceof C){
   echo "\$b implements interface c";
}else {
   echo "\$b don't implements interface c";
}
?>
```
##### 类提示的思想
指定参数传入的类型，如果不是传入该类型参数将报错；
```
function check_hint(B $someclass){}; //如果传入的参数类型不是B的实例，将报错;
```
##### 延迟静态绑定
```
//输出的结果为 who method of class B;
<?php
class A{
   public static function who(){
     echo "who method of class A";
   }

   public static function test(){
     echo static::who(); //here comes later static binding;
   }
}

class B extends A{
   public static function who(){
     echo "who method of class B";
   }
}
B::test();
?>
```
##### call方法
 call方法用来监控一个对象中的其他方法，如果试着调用一个对象中不存在或被权限控制中的方法，__call方法将被自动调用;
```
<?php
class A{
  public function __call($name,$arguments){
     echo "call of method is =>".$name." <br/>";
     echo "arguments is".implode(",",$arguments)."<br/>";
  }
}
$a = new A();
$a->callMe("bac","123","www");
?>
```
##### 类转换为字符串
```
class A{
public $name;
function __toString(){
 return (var_export($this,TRUE));
}
}
$a = new A();
$a->name = "zhangsan";
$a->__toString();
```


