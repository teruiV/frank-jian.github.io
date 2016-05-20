---
title: PHP对数据库的增删改查
date: 2016-05-06 17:53:07
tags: [PHP]
categories: PHP
---
对于PHP和前端HTML的CURD交互操作的详情请参考链接资料，写的非常详细，这里主要总结一下，PHP的CURD的操作;
```
//数据库配置
$ip = 'localhost';
$user = 'root';
$password = 'root';
$database = 'test';

//增
$isbn = "A001";
$author = "jianwl";
$title = "PHP";
$price = 222;

@$db = new mysqli($ip,$user,$password,$database);
if(mysqli_connect_errno()){
	echo "mysql connected error";
    exit();
}

$sql_insert = "INSERT INTO books(isbn,author,title,price) VALUES('$isbn','$author','$title','$price')";
mysql_query($sql_insert);
echo "insert into db 1 data.....";

//查

$sql_select = "SELECT * FROM books";
$result_set = mysql_query($sql_select);
while($row=mysql_fetch_row($result_set)){
echo "isbn = ".$row['isbn']." author = ".$row['author']." title = ".$row['title']." price = ".$row['price']."<br/>" ;
}

//改
$sql_update = "update books SET title="MYSQL PHP CONNECT WHERE isbn='$isbn'";
mysql_query($sql_update);
echo "data has updated";

//删
$sql_delete = "delete books where isbn=dd";
mysql_query($sql_delete);
echo "data has deleted";


```


参考资料：
[Simple PHP CRUD Operations with MySQL](http://www.codingcage.com/2014/12/simple-php-crud-operations-with-mysql.html) 
