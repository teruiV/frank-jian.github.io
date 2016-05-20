---
title: Java设计模式之策略模式
date: 2016-05-10 11:37:45
tags: [JAVA]
categories: JAVA
---
###### 设计原则
> 1、逻辑代码独立到单独的方法中,注重封装性-易读，易复用;
> 2、写类、写方法、写功能时，应考虑其移植性，复用性，防止一次性代码；
> 3、熟练运用继承的思想:找出应用中相同之处，且不容易发生变化的东西，把它们抽取到抽象类中，让子类去继承它们；
> 4、熟练运用接口的思想：找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代码混在一起;

通过以下示例，我们来感受一下设计模式的好处;
**场景：**
模拟鸭子游戏的应用程序，要求，游戏中的会出现各种颜色外形的鸭子，一边游戏洗水，一边呱呱叫。

###### 实现1：一次性代码
```
直接编写出各种鸭子的类：MallardDuck,RedHeadDuck各个类有三个方法：
quack() 叫的方法；
swim()  游水的方法；
display()： 外形的方法;
```
###### 实现2：继承
设计一个鸭子的超类，并让各个鸭子继承这个超类,将其中共同的部分提取出来，避免重复编程;
```
public abstract class Duck{
	public void quack(){
    	System.out.println("呱呱叫");
    }
    public void swim(){
    	System.out.println("游泳");
    }
    public abstract display(); //外观不一样，由子类决定
}

public class MallardDuck extends Duck{
	public void display(){
    	System.out.println("野鸭的颜色是绿色的");
    }
}

public class RedHeadDuc extends Duck{
	public void display(){
    	System.out.println("红头鸭的颜色是红色的");
    }
}
```
不幸的是，现在客户又提出了新的需求，想让鸭子飞起来。这个对于OO程序员，在简单不过了，在超类中在加一个方法就可以了。
```
public class Duck{
	public void quack(){
    	System.out.println("呱呱叫");
    }
    public void swim(){
    	System.out.println("游泳");
    }
    public void fly(){
    	System.out.println("飞啦");
    }
    public abstract display(); //外观不一样，由子类决定
}

//对于不能飞的鸭子，在子类中只需简单覆盖
public class DisabledDuck extends Duck{
	public void display(){
    	System.out.println("残废鸭的颜色.");
    }
    public void fly(){
		//覆盖，什么事都不做
    }
}
```
实现2点评
对于上面的设计，你可能会发现一些弊端，如果超类有新的特性，子类都必须变动，这是我们开发最不喜欢看到的，故继承耦合性太高了。
###### 实现3:接口
我们把容易变化的部分提取出来并封装，来应付以后的变化，虽然代码量加大了，但可用性提高了，耦合度也降低了。
```
public interface Flyable(){
	public void fly();
}

public interface Quackable(){
	public void quack();
}

public class Duck(){
	public void swim(){
    	System.out.println("游泳");
    }
    public abstract void display();
}

public class MallardDuck extends Duck implements Flyable,Quackable{
	public void display(){
    	System.out.println("野鸭的颜色是绿色的");
    }
    public void fly(){
    	System.out.println("飞啦");
    }
    public void quack(){
    	System.out.println("呱呱叫");
    }
}

public class DisabledDuck extends Duck implements Quackable{
	public void display(){
    	System.out.println("能叫不能飞的残废鸭颜色....");
    }
    public void quack(){
    	System.out.println("呱呱叫");
    }
}
```
实现3接口点评
好处是，这样的设计，降低了程序之间的耦合度；
缺点是，Flyable和Quackable接口一开始似乎还挺不错的，解决了问题，只有会飞的鸭子才实现Flyable,但是Java接口不具有实现代码，所以实现接口无法达到的复用。

###### 实现四，策略模式
为了要分开变化和不变化的部分，准备建立两组类,一个是Fly相关的，另一个是Quack相关的，每组类实现各自的动作。
```
public interface FlyBehavior{
	public void fly();
}

public interface QuackBehavior{
	public void quack();
}
public class FlyWithWings implements FlyBehavior{
	public void fly(){
    	System.out.println("飞啦");
    }
}
public class FlyNoWay implements FlyBehavior{
	public void fly(){
    	System.out.println("什么都不做，不会飞啊");
    }
}

public class Quack implements QuackBehavior{
	public void quack(){
    	System.out.println("呱呱叫");
    }
}

public class MuteQuack implements QuackBehavior{
	public void quack(){
    	System.out.println("什么都不做，不会叫");
    }
}
```
这样的设计可以让飞行和呱呱叫的动作被其他的对象复用，因为这些行为已经和鸭子类无关了，而我们增加一些新的行为，不会影响到既有的行为类，也不会影响使用到的飞行行为的鸭子类；
```
public abstract class Duck {
	FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;
    
    public Duck(){}
   
    public void swim(){
    	System.out.println("游泳");
    }
    
    public abstract void display();
    
    public void performFly(){
    	flyBehavior.fly();
    }
    
    public void performQuack(){
    	quackBehavior.quack();
    }

}

public MallardDuck extends Duck{
	public MallardDuck(){
    	this.flyBehavior = new FlyWithWings();
        this.quackBehavior = new MuteQuack();
    }
    
    public void display(){
    	System.out.println("鸭子是绿色的");
    }
}
```
实现4点评
即实现了解耦又实现了代码的复用；

###### 策略模式定义
定义了算法族，分别封装起来，让它们之间可以相互替换，此模式让算法的变化独立于使用算法的客户。