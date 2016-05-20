---
title: PHP数组
date: 2016-05-04 09:59:30
tags: [PHP]
categories: PHP
---
##### 创建数组
```
<?php
//初始化数组
$products = array('tires','oil','spark');
//初始化关联数组
$prices = array('tires'=>100,'oil'=>10,'spark'=>4);
foreach($products as $product){
  echo "product = " . $product. " - price = " . $prices[$product] . "</br> ";
}
?>
```
##### 循环
```
<?php
//初始化数组
$products = array('tires','oil','spark');
//初始化关联数组
$prices = array('tires'=>100,'oil'=>10,'spark'=>4);

#关系型数组循环方式1
while(list($product,$price) = each($prices)) {
  echo "(1) - $product - $price </br>";
}
#关系型数组循环方式2
foreach($prices as $key => $value) {
  echo " (2) - ". $key." - ".$value." </br>";
}

#关系型数组循环方式3
reset($prices);
while($element = each($prices)){
   echo " (3)- ". $element['key'].' - ' .$element['value'].'</br>';
}

#特别注意，使用each函数将记录当前元素，如果脚本中两次使用该数组，应该使用reset（）重置元素至开始处
reset($prices);
while($element = each($prices)){
   echo " (4) -". $element['key'].' - ' . $element['value'].'</br>';
}

# 数组循环
foreach($products as $product){
  echo "[1] - product = ".$product ."</br>";
}
```
##### 多维数组
```
$products = array(
	array('code' =>'CODE'),
    array("price"=>10)
);

for($row = 0; $row < 3; $row ++) {
   while( list($key,$value) = each($products[$row])) {
   		echo $key ."  - " .$value ."</br>";
   }
}
```
数组排序
```
<?php
$products = array('fruit','apple','banana');
$prices = array(100,79,88);

#字符串升序
sort($products);
foreach($products as $product){
 echo  $product." ";
}
echo "</br>";

#数字升序
sort($prices);
foreach($prices as $price){
  echo $price." " ;
}
# 反向排序 rsort($products);

echo "</br>";

#关联数组排序
$prices = array('fruit'=> 5,'apple'=>20,'banana'=>3);

#按价格升序排序
asort($prices);
while(list($product,$price) = each($prices)){
  echo $product . ' - ' .$price . "</br>";
}

#按水果名升序排序
ksort($prices);
while(list($product,$price) = each($prices)){
  echo $product . ' - ' .$price . "</br>";
}
#降序排 arsort($price)、 krsort($price)

echo "<==></br>";
#多维数组排序,按价格升序
$products = array(
array('fruit','FRUIT',20),
array('apple','APPLE',5),
array('banana','BANANA',3)
);
function compare($x,$y) {
  if($x[2] == $y[2]){
    return 0;
  } else if($x[2] < $y[2]){
    return -1;
  } else {
    return 1;
 }
}
usort($products,'compare');
for($row = 0;$row < 3;$row ++){
for($column = 0; $column < 3; $column ++){
    echo $products[$row][$column] . ' ';
  }
echo "</br>";
}

#对数组重新排序
$numbers = array(1,2,3,4,5,6,7,8,9);
#对数组重新排序,应用场景每次登陆时，看到不一样的三张图片;
shuffle($numbers);
foreach($numbers as $number){
 echo $number." ";
}
echo "</br>";
#对数组反序
$numbers = array(1,3,5,7,4);
$numbers = array_reverse($numbers);
foreach($numbers as $number){
 echo $number.' ';
}

echo "</br>";
#产生1-10,步长为2，数组;
$numbers = range(1,10,2);
foreach($numbers as $number){
 echo $number.' ';
}

echo "</br>";
$products = file("./fruit.txt");
echo "count = ". count($products).'</br>';
foreach($products as $product){
   #分割字符串
   $array = explode("\t",$product);
   echo 'price = '.$array[2].'</br>';
  
}

#对每个元素应用任何函数
$numbers = array(1,2,3,4);
function my_print($value){
  echo "$value <br/>";
}
array_walk($numbers,'my_print');

#统计数组元素个数count\sizeof()
$array = array(1,2,3,2,1,1,1,1,1,1,6,7);
echo "count = " . count($array).' sizeof = ' .sizeof($array)."</br>";

#统计数组中，每个元素出现的次数;
$array = array_count_values($array);
while(list($key,$value) = each($array)){
  echo $key . ' - '.$value."</br>";
}

#将数组转换成标量变量，输出变量的名称，必须是数组中变量的名称；
$products = array('apple'=>5,'banana'=>3);
extract($products,EXTR_OVERWRITE);
echo "apple_price = $apple , banana_price = $banana";

extract($products,EXTR_PREFIX_ALL,"my_prefix");
echo "apple_price = $my_prefix_apple,banana_price = $my_prefix_banana";

#extract_type常用值
#EXTR_OVERWRITE 当关键字发生冲突时，覆盖已有的关键字；
#EXTR_PREFIX_ALL 给关键字加前缀;

?>

```


##### 踩坑提示
```
一行代码结束，别忘了分号（；）
引用变量，别忘了$

```
