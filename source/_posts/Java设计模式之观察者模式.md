---
title: Java设计模式之观察者模式
date: 2016-05-10 15:56:05
tags: [JAVA]
categories: JAVA
---
###### 摘要
观察者模式(ObservePattern),用于实时监测某些Object的动态，只要Object一改变，那么他的所有观察者Observer都会被通知到，之后观察者会根据Object的改变进行下一步操作。
###### 问题的引出
要实现一个天气预报的功能，当天气的数据发生变化的时候，会实时的以三种形式来显示天气，当前天气情况、统计分析情况、天气预报。这里我们不管数据如何来，就假设我们已经能够获取到数据了，在程序中通过调用被观察的对象的notifyObserver()方法来通知所有观察者；
###### 问题分析
1. 通过分析我们得出一个简单的结论，天气数据一旦更新，那么就要实时的改变显示，观察者模式可以很好的解决这种模型问题。
2. 既然确定观察者模型，就要定位出谁是观察者、谁是被观察者、这里有个简单的原则、观察者和被观察者之间的关系是多对一的关系，也就是说被观察者只有一个就是我们的天气数据信息，观察者则是三种要显示不同的终端;
3. 每个观察者都要动态的显示信息，所以我们应该
4. 考虑到可扩展性、低耦合性、灵活性和对扩展开放、对修改关闭的原则和面向接口编程，我们下面具体类的设计
5. 根据角色我们可以抽象三个接口：
	a) 所有被观察者的接口 Subject
    b) 所有观察者的接口 Observer
    c) 显示信息的接口 DisplayElement
6. Subject拥有三个关于Observer的方法、注册、移除和通知Observer的方法(观察者肯定要和被观察者集合起来)
7. 对于Observer肯定要有一个udpate方法，一旦监测要Subject有变动，就要更新信息，所以还要实现DisplayElement接口。

###### 具体实现
1、设计Subject接口
```
public interface Subject {
    public void registerObserver(Observer observer);
    public void removeObserver(Observer observer);
    public void notifyObserver();
}
```
2、设计Observer接口
```
public interface Observer {
    public void update(float temperature,float humidity,float pressure);
}
```
3、设计DisplayElement接口
```
public interface DisplayElement{
 	public void display();
}
```
4、Subject接口的被观察者  --- WeatherDate
```
public class WeatherDate implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private float temperate;
    private float humidity;
    private float pressure;

    @Override
    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        int index = observers.indexOf(observer);
        if(index > 0){
            observers.remove(observer);
        }
    }

    @Override
    public void notifyObserver() {
        for(int i=0; i < observers.size(); i++){
            Observer o = (Observer) observers.get(i);
            o.update(temperate,humidity,pressure);
        }
    }

    /**
     * 模仿数据变动时，自动触发notifyObserver函数
     */
    public void measurementsChanged(){
        notifyObserver();
    }

    /**
     * 模仿数据变动、即当我们调用这个方法时就说明数据有变化，这样所有的观察者都会被通知；
     */
    public void setMeasurements(float temperate,float humidity,float pressure){
        this.temperate = temperate;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }
}
```
5、观察者Observer的具体实现
```
public class CurrentConditionDisplay implements DisplayElement,Observer {
    private float temperature;
    private float humidity;
    private float pressure;
    private Subject weatherDate;

    public CurrentConditionDisplay(Subject weatherDate){
        this.weatherDate = weatherDate;
        this.weatherDate.registerObserver(this);
    }

    @Override
    public void display() {
        System.out.println( CurrentConditionDisplay.class.getName() + " temperature = " + temperature + "  humidity =   " + humidity + " pressure = " + pressure);
    }

    @Override
    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        display();
    }
}
```
6、ForecastConditionDisplay 与CurrentConditionDisplay实现类似
7、Client测试
```
public class Client {
    public static void main(String[] args) {
        WeatherDate subject = new WeatherDate();

        ForecastConditionDisplay condition1 = new ForecastConditionDisplay(subject);
        CurrentConditionDisplay condition2 = new CurrentConditionDisplay(subject);
        subject.setMeasurements(10,20,30);
    }
}
```
###### Java内置观察者模式
WeatherData类似于自己实现观察者模式中的的Subject的具体实现，继承了java提供的java.util.Observable，Observable封装了之前subject提供的所有方法;
```
import java.util.Observable;

/**
 * Subject
 * User: jianwl
 * Date: 2016/5/11
 * Time: 11:16
 */
public class WeatherData extends Observable{
    private float temperature;
    private float humidity;
    private float pressure;

    public  WeatherData(){}

    public void messureChanged(){
        setChanged();
        notifyObservers();
    }

    public  void setMessurements(float temperature,float humidity,float pressure){
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        messureChanged();
    }

    public float getTemperature() {
        return temperature;
    }

    public float getHumidity() {
        return humidity;
    }

    public float getPressure() {
        return pressure;
    }

}
```
CurrentConditionsDisplay,类似于自己实现观察者模式中的Observer的具体实现，实现了java提供的java.util.Observer的接口和另一个自己创建的DisplayElement接口;
```
public interface DisplayElement {
    public void display();
}


import java.util.Observable;
import java.util.Observer;

/**
 * User: jianwl
 * Date: 2016/5/11
 * Time: 11:23
 */
public class CurrentConditionsDisplay implements Observer,DisplayElement {
    Observable observable;
    private float temperature;
    private float humidity;

    public CurrentConditionsDisplay(Observable observable) {
        this.observable = observable;
        observable.addObserver(this);
    }

    @Override
    public void display() {
        System.out.println("CurrentConditionsDisplay  temperature = " + temperature + " humidity = " + humidity );
    }

    @Override
    public void update(Observable o, Object arg) {
        if(o instanceof WeatherData){
            WeatherData weatherData = (WeatherData)o;
            this.temperature = weatherData.getTemperature();
            this.humidity = weatherData.getHumidity();
            display();
        }
    }
}
```
测试Java内置的观察者模式,XFireConditionsDisplay、ForeConditionsDisplay这两个类的实现和CurrentConditionsDisplay类似;
同时通过测试结果可知，Java内置的观察者模式，通知的顺序是先入后出的；
```
public class Client {
    //通知的顺序是先入后出的；
    public static void main(String[] args) {
        WeatherData weatherData = new WeatherData();
        CurrentConditionsDisplay condition1 = new CurrentConditionsDisplay(weatherData);

        XFireConditionsDisplay condition2 = new XFireConditionsDisplay(weatherData);
        ForeConditionsDisplay condition3 = new ForeConditionsDisplay(weatherData);

        weatherData.setMessurements(22,11,11);
    }
}

```
###### 总结
Java内置的观察者模式有什么问题？
1、Observable是一个“类”，你必须设计一个类继承它，如果某类想同时具有Observable类和另一个超类的行为，就会陷入两难，毕竟Java不支持多重继承。限制了Observable的复用的能力；
2、Observable将关键的方法保护起来了，setChanged()方法被保护起来了（被定义成protected）,意味着除非你继承自Obserable，否则你无法创建Obserable实例并组合到自己的对象中来；
