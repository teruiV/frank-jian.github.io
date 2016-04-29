---
title: Java注解
date: 2016-04-20 10:28:14
tags: [JAVA]
categories: JAVA
---
## 什么是注解？
> Annotations, a form of metadata, provide data about a program that is not part of the program itself. Annotations have no direct effect on the operation of the code they annotate.

注解是JAVA 5的一个新特性，它相当于是一种嵌入在程序中的元数据
## 使用场景
>- Information for the compiler — Annotations can be used by the compiler to detect errors or suppress warnings.
- Compile-time and deployment-time processing — Software tools can process annotation information to generate code, XML files, and so forth.
- Runtime processing — Some annotations are available to be examined at runtime.

运用场景主要是三个方面：
-  内置注解，用于告诉编译器，哪些方法被覆盖，哪些方法忽略警告；

```
//内置注解举例说明
@Override
当我们想要覆盖父类的一个方法时，需要使用该注解告知编译器，我们正在覆盖一个方法。这样的话，当父类的方法被删除或修改了，编译器会提示错误信息。

@Deprecated
当我们想要让编译器知道一个方法已经被弃用时，应该使用这个注解。

@SuppressWarning
告知编译器，忽略他们产生的特殊警告
```
- 用注解解析工具对其进行解析
- 注解可以指定在运行时有效

## Java自定义注解
```
package com.journaldev.annotations;
 
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
 
@Documented
@Target(ElementType.METHOD)
@Inherited
@Retention(RetentionPolicy.RUNTIME)
public @interface MethodInfo{
    String author() default "Pankaj";
    String date();
    int revision() default 1;
    String comments();
}
```
- 注解方法不能有参数
- 注解方法的返回类型局限于基本类型，字符串，枚举
- 注解方法可以包含默认值
- 注解可以包含与其绑定的元注解，元注解为注解提供信息，有四种元注解类型

1. @Documented – 表示使用该注解的元素应被javadoc或类似工具文档化，它应用于类型声明，类型声明的注解会影响客户端对注解元素的使用。如果一个类型声明添加了Documented注解，那么它的注解会成为被注解元素的公共API的一部分。

2. @Target – 表示支持注解的程序元素的种类，一些可能的值有TYPE, METHOD, CONSTRUCTOR, FIELD等等。如果Target元注解不存在，那么该注解就可以使用在任何程序元素之上。

3. @Inherited – 表示一个注解类型会被自动继承，如果用户在类声明的时候查询注解类型，同时类声明中也没有这个类型的注解，那么注解类型会自动查询该类的父类，这个过程将会不停地重复，直到该类型的注解被找到为止，或是到达类结构的顶层（Object）。

4. @Retention – 表示注解类型保留时间的长短，它接收RetentionPolicy参数，可能的值有SOURCE, CLASS, 以及RUNTIME。

## Java注解解析

我们将使用Java反射机制从一个类中解析注解，请记住，注解保持性策略应该是RUNTIME，否则它的信息在运行期无效，我们也不能从中获取任何数据。
```
package com.journaldev.annotations;
 
import java.io.FileNotFoundException;
import java.util.ArrayList;
import java.util.List;
 
public class AnnotationExample {
 
    @Override
    @MethodInfo(author = "Pankaj", comments = "Main method", date = "Nov 17 2012", revision = 1)
    public String toString() {
        return "Overriden toString method";
    }
 
    @Deprecated
    @MethodInfo(comments = "deprecated method", date = "Nov 17 2012")
    public static void oldMethod() {
        System.out.println("old method, don't use it.");
    }
 
    @SuppressWarnings({ "unchecked", "deprecation" })
    @MethodInfo(author = "Pankaj", comments = "Main method", date = "Nov 17 2012", revision = 10)
    public static void genericsTest() throws FileNotFoundException {
        List l = new ArrayList();
        l.add("abc");
        oldMethod();
    }
 
}

package com.journaldev.annotations;
 
import java.lang.annotation.Annotation;
import java.lang.reflect.Method;
 
public class AnnotationParsing {
    public static void main(String[] args) {
        try {
            for (Method method : AnnotationParsing.class
                    .getClassLoader()
                    .loadClass(("com.journaldev.annotations.AnnotationExample"))
                    .getMethods()) {
                // checks if MethodInfo annotation is present for the method
                if (method
                        .isAnnotationPresent(com.journaldev.annotations.MethodInfo.class)) {
                    try {
                        // iterates all the annotations available in the method
                        for (Annotation anno : method.getDeclaredAnnotations()) {
                            System.out.println("Annotation in Method '"
                                    + method + "' : " + anno);
                        }
                        MethodInfo methodAnno = method
                                .getAnnotation(MethodInfo.class);
                        if (methodAnno.revision() == 1) {
                            System.out.println("Method with revision no 1 = "
                                    + method);
                        }
                    } catch (Throwable ex) {
                        ex.printStackTrace();
                    }
                }
            }
        } catch (SecurityException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```