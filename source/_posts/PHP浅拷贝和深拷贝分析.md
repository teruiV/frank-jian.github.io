title: PHP浅拷贝和深拷贝分析
date: 2016-05-03 12:03:16
tags: [PHP]
categories: PHP
---
在PHP中，对象的赋值实际上是引用操作，比如
```
<?php
class person{
 public $name;
}
$person1 = new person();
$person1->name = "zhangsan";
$person2 = $person1;
$person2->name = "lisi";

echo "person1->name = ".$person1->name." person2->name = ".$person2->name;
?>
```
因为person1和person2都只是指向同一个内存区的引用,所以修改任何一个对象都会同时修改另外一个对象.

在有些时候，我们其实并不希望这种引用的赋值方式，我们希望能完全复制一个对象，这时候就需要用到php中的clone（对象的复制);
```
<?php
class person{
 public $name;
}
$person1 = new person();
$person1->name = "zhangsan";
$person2 = clone $person1;
$person2->name = "lisi";

echo "person1->name = ".$person1->name." person2->name = ".$person2->name;
?>
```
因为clone的方式实际上是对整个对象的内存区域，进行了一次复制并用新的对象变量指向新的内存，因此赋值后的对象和源对象之间基本上说是独立的。

这是什么意思？因为PHP中的对象拷贝采用的是浅拷贝的方法，如果对象里的属性成员本身就是引用类型的，clone以后这些成员并没有真正被复制，仍然是引用。
```
<?php
class address{
 public $palace;
 public $no;
}

class person{
public $name;
public $address;
}

$address = new address();
$address->palace = "shanghai";
$address->no = 21;

$person1 = new person();
$person1->name = "zhangsan";
$person1->address = $address;

$person2 = clone $person1;

echo "before modify person1 name = ".$person1->name." palace = ".$person1->address->palace." <br/>person2 name = ".$person2->name." person2 palace = ".$person2->address->palace. "<br/>";

echo "=====================================<br/>";

$person2->address->no = 33;
$person2->address->palace = "beijing";
$person2->name = "lisi";

echo "aflter modify person1 name = ".$person1->name." palace = ".$person1->address->palace." <br/>person2 name = ".$person2->name." person2 palace = ".$person2->address->palace. "<br/>"; //结果为 person1->address->palace = "beijing", person2->address->palace = "beijing";
?>
```
一般来讲，你用clone来复制对象，希望把两个对象彻底分开，不希望他们之间有任何关联，但由于clone的浅拷贝的特性，有时候会出现非期望的结果，就像上面的例子那样。
$person1 ->address = $address;这句话是引用类型的赋值，person1->address和person2->address 实际上是指向同一个内存区对象的数据引用，因此修改其中任何一个都会影响其他两个。

如何解决这个问题呢？以下采用两种方法来解决：
- 在上面的代码中，添加address的对象拷贝即可，但治标不治本;

```
$person2->address = clone $person1->address;
```

- 采用PHP中的__clone方法把浅拷贝转换为深拷贝

```
<?php
class address{
 public $palace;
 public $no;
}

class person{
public $name;
public $address;

public function __clone(){
 $this->address = clone $this->address; //深拷贝的引用;
}

}

$address = new address();
$address->palace = "shanghai";
$address->no = 21;

$person1 = new person();
$person1->name = "zhangsan";
$person1->address = $address;

$person2 = clone $person1;
echo "before modify person1 name = ".$person1->name." palace = ".$person1->address->palace." <br/>person2 name = ".$person2->name." person2 palace = ".$person2->address->palace. "<br/>";

echo "=====================================<br/>";
$person2->address->no = 33;
$person2->address->palace = "beijing";
$person2->name = "lisi";

echo "aflter modify person1 name = ".$person1->name." palace = ".$person1->address->palace." <br/>person2 name = ".$person2->name." person2 palace = ".$person2->address->palace. "<br/>";
?>
```







