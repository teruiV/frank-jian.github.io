---
title: PHP常用类和接口的判断方法
date: 2016-05-06 10:11:18
tags: [PHP]
categories: PHP
---
```
<?php
        class Computer {
                public function _run() {}
                private function _go() {}
                public $_name = 'dell';
                private $_model = 'i7';
        }

        class NoteComputer extends Computer {

        }

        interface Person {

        }
        $computer = new Computer();
        $notecomputer = new NoteComputer();

        //1.检查类是否存在
        echo class_exists('Computer');
        //2.获取对象的类名
        echo get_class($computer);
        //3.获取类中公共的方法
        print_r(get_class_methods($computer));
        //4.获取类中的字段
        print_r(get_class_vars('Computer'));
        //5.获取子类的父类
        echo get_parent_class($notecomputer);
        //6.判断接口是否存在
        echo interface_exists('Person');
        //7.判断对象是否是这个类，$notecomputer的类的父类是Computer
        echo is_a($notecomputer,'Computer');
        //8.判断对象是否是类的子类。
        echo is_subclass_of($notecomputer,'Computer');
        //9.判断对象是否有这个方法
        echo method_exists($computer,'_run');
?>
```