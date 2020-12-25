---
title: 创建型模式
date: 2020/12/19 11:45
categories:
	- [计算机,设计模式]
tags:
	- 设计模式 
---



### 工厂模式

> 目的：定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。
>
> 解决：主要解决接口选择的问题。
>
> 应用实例：您需要一辆汽车，可以直接从工厂里面提货，而不用去管这辆汽车是怎么做出来的，以及这个汽车里面的具体实现。

1. 创建一个汽车接口

> car.java

```java
public interface Car {
    void product();
}
```

1. 创建实现汽车接口的实体类

> AudiCar.java

```java
public class AudiCar implements Car{
    @Override
    public void product() {
        System.out.println("生产奥迪汽车");
    }
}
```

> BenzCar.java

```java
public class BenzCar implements Car{
    @Override
    public void product() {
        System.out.println("生产奔驰汽车");
    }
}
```

> AudiCar.java

```java
public class BmwCar implements Car{
    @Override
    public void product() {
        System.out.println("生产宝马汽车");
    }
}
```

1. 创建一个工厂，生成基于给定信息的实体类的对象。

> CarFactory.java

```java
public class CarFactory {
    //通过getCar获得汽车类型的对象
    public Car getCar(String carType){
        if (carType == null){
            return null;
        }
        if (carType.equals("Audi")){
            return new AudiCar();
        }
        if (carType.equals("Bmw")){
            return new BmwCar();
        }
        if (carType.equals("Benz")){
            return new BenzCar();
        }
        return null;
    }
}
```

1. 使用该工厂，通过传递类型信息来获取实体类的对象。

> FactoryPatternDemo

```java
public class FactoryPatternDemo {
    public static void main(String[] args) {
      CarFactory carFactory = new CarFactory();
      Car adui = carFactory.getCar("Audi");
      adui.product();
      Car bmw = carFactory.getCar("Bmw");
      bmw.product();
      Car benz = carFactory.getCar("Benz");
      benz.product();
    }
}
```

1. 输出结果

> 生产奥迪汽车 生产宝马汽车 生产奔驰汽车

### 抽象工厂模式

> 目的：提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。
>
> 解决：主要解决接口选择的问题。
>
> 应用实例：对于身上的穿搭，每一件成套的衣服都包括具体的衣服（比如长袖、短袖）、具体的裤子（比如牛仔裤、休闲裤），这些具体的衣服也都是衣服（抽象产品），这些具体的裤子也是裤子（另一种抽象产品）

1. 创建一个形状接口

> Shape.java

```java
public interface Shape {
    void draw();
}
```

1. 创建实现形状接口的实体类

> Rectangle.java

```java
public class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("画一个矩形");
    }
}
```

> Square.java

```java
public class Square implements Shape{
    @Override
    public void draw() {
        System.out.println("画一个正方形");
    }
}
```

> Circle.java

```java
public class Circle implements Shape{
    @Override
    public void draw() {
        System.out.println("画一个圆形");
    }
}
```

1. 创建一个颜色接口

> Color.java

```java
public interface Color {
    void fill();
}
```

1. 创建实现颜色接口的实体类

> Blue.java

```java
public class Blue implements Color {
    @Override
    public void fill() {
        System.out.println("填充蓝色");
    }
}
```

> Red.java

```java
public class Red implements Color{
    @Override
    public void fill() {
        System.out.println("填充红色");
    }
}
```

> Green.java

```java
public class Green implements Color{
    @Override
    public void fill() {
        System.out.println("填充绿色");
    }
}
```

1. 为 Color 和 Shape 对象创建抽象类来获取工厂

> AbstractFactory.java

```java
public abstract class AbstractFactory {
    public abstract Color getColor(String color);
    public abstract Shape getShape(String shape) ;
}
```

1. 创建扩展了 AbstractFactory 的工厂类，基于给定的信息生成实体类的对象。

> ShapeFactory.java

```java
public class ShapeFactory extends AbstractFactory {
    @Override
    public Color getColor(String color) {
        return null;
    }
    @Override
    public Shape getShape(String shape) {
        if (null == shape){
            return null;
        }
        else if ("Circle".equals(shape)){
            return new Circle();
        }
        else if ("Square".equals(shape)){
            return new Square();
        }
        else if ("Rectangle".equals(shape)){
            return new Rectangle();
        }
        return null;
    }
}
```

> ColorFactory.java

```java
public class ColorFactory extends AbstractFactory {
    @Override
    public Color getColor(String color) {
        if (null == color){
            return null;
        }
        else if ("Red".equals(color)){
            return new Red();
        }
        else if("Blue".equals(color)){
            return new Blue();
        }
        else if ("Green".equals(color)){
            return new Green();
        }
        return null;
    }
    @Override
    public Shape getShape(String shape) {
        return null;
    }
}
```

1. 创建一个工厂创造器/生成器类，通过传递形状或颜色信息来获取工厂。

> FactoryProducer.java

```java
public class FactoryProducer {
    public static AbstractFactory getFactory(String choice){
        if ("Shape".equals(choice)){
            return new ShapeFactory();
        }
        else if ("Color".equals(choice)){
            return new ColorFactory();
        }
        return null;
    }
}
```

1. ​	使用 FactoryProducer 来获取 AbstractFactory，通过传递类型信息来获取实体类的对象。

> AbstractFactoryPatternDemo.java

```java
public class AbstractFactoryPatternDemo {
    public static void main(String[] args) {
        //获取形状工厂
        AbstractFactory shapeFactory = FactoryProducer.getFactory("Shape");
        //获取形状为Circle的对象
        Shape circle = shapeFactory.getShape("Circle");
        circle.draw();
        //获取形状为Rectangle的对象
        Shape rectangle = shapeFactory.getShape("Rectangle");
        rectangle.draw();
        //获取形状为Square的对象
        Shape square = shapeFactory.getShape("Square");
        square.draw();
        //获取颜色工厂
        AbstractFactory colorFactory = FactoryProducer.getFactory("Color");
        //获取颜色为Red的对象
        Color red = colorFactory.getColor("Red");
        red.fill();
        //获取颜色为Blue的对象
        Color blue = colorFactory.getColor("Blue");
        blue.fill();
        //获取颜色为Green的对象
        Color green = colorFactory.getColor("Green");
        green.fill();
    }
}
```

1. 输出结果

> 画一个圆形 画一个矩形 画一个正方形 填充红色 填充蓝色 填充绿色

### 单例模式

> 目的：保证一个类仅有一个实例，并提供一个访问它的全局访问点。
>
> 解决：一个全局使用的类频繁地创建与销毁。
>
> 应用实例：Windows 是多进程多线程的，在操作一个文件的时候，就不可避免地出现多个进程或线程同时操作一个文件的现象，所以所有文件的处理必须通过唯一的实例来进行。

1. 懒汉式，线程不安全

> 描述：这种方式是最基本的实现方式，这种实现最大的问题就是不支持多线程。因为没有加锁 synchronized，所以严格意义上它并不算单例模式。 这种方式 lazy loading 很明显，不要求线程安全，在多线程不能正常工作。

```java
public class Singleton {
    private static Singleton instance;
    private Singleton (){}
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

1. 懒汉式，线程安全

> 描述：这种方式具备很好的 lazy loading，能够在多线程中很好的工作，但是，效率很低，99% 情况下不需要同步。
>
> 优点：第一次调用才初始化，避免内存浪费。
>
> 缺点：必须加锁 synchronized 才能保证单例，但加锁会影响效率。
>
> getInstance() 的性能对应用程序不是很关键（该方法使用不太频繁）。

```java
public class Singleton {
    private static Singleton instance;
    private Singleton (){}
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

1. 饿汉式

> 描述：这种方式比较常用，但容易产生垃圾对象。
>
> 优点：没有加锁，执行效率会提高。
>
> 缺点：类加载时就初始化，浪费内存。
>
> 它基于 classloader 机制避免了多线程的同步问题，不过，instance 在类装载时就实例化，虽然导致类装载的原因有很多种，在单例模式中大多数都是调用 getInstance 方法， 但是也不能确定有其他的方式（或者其他的静态方法）导致类装载，这时候初始化 instance 显然没有达到 lazy loading 的效果。

```java
public class Singleton {
    private static Singleton instance = new Singleton();
    private Singleton (){}
    public static Singleton getInstance() {
        return instance;
    }
}
```

1. 双检锁/双重校验锁（DCL，即 double-checked locking）

> 描述：这种方式采用双锁机制，安全且在多线程情况下能保持高性能。 getInstance() 的性能对应用程序很关键。

```java
public class Singleton {
    private volatile static Singleton singleton;
    private Singleton (){}
    public static Singleton getSingleton() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}
```

1. 登记式/静态内部类

> 描述：这种方式能达到双检锁方式一样的功效，但实现更简单。对静态域使用延迟初始化，应使用这种方式而不是双检锁方式。这种方式只适用于静态域的情况，双检锁方式可在实例域需要延迟初始化时使用。

```java
public class Singleton {
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    private Singleton (){}
    public static final Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

1. 枚举

> 描述：这种实现方式还没有被广泛采用，但这是实现单例模式的最佳方法。它更简洁，自动支持序列化机制，绝对防止多次实例化。

```java
public enum Singleton {
    INSTANCE;
    public void whateverMethod() {}
}
```

**一般情况下，不建议使用第 1 种和第 2 种懒汉方式，建议使用第 3 种饿汉方式。只有在要明确实现 lazy loading 效果时，才会使用第 5 种登记方式。如果涉及到反序列化创建对象时，可以尝试使用第 6 种枚举方式。如果有其他特殊的需求，可以考虑使用第 4 种双检锁方式。**

### 建造者模式

> 目的：将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。
>
> 解决：有时候面临着"一个复杂对象"的创建工作，其通常由各个部分的子对象用一定的算法构成；由于需求的变化，这个复杂对象的各个部分经常面临着剧烈的变化，但是将它们组合在一起的算法却相对稳定。
>
> 应用实例：去肯德基，汉堡、可乐、薯条、炸鸡翅等是不变的，而其组合是经常变化的，生成出所谓的"套餐"。

1. 创建一个表示食物条目和食物包装的接口。

> Item.java

```java
public interface Item {
    public String name();
    public Packing packing();
    public float price();    
}
```

> Packing.java

```java
public interface Packing {
    public String pack();
}
```

1. 创建实现 Packing 接口的实体类。

> Wrapper.java

```java
public class Wrapper implements Packing {
    @Override
    public String pack() {
        return "纸质包装";
    }
}
```

> Bottle.java

```java
public class Bottle implements Packing {
    @Override
    public String pack() {
        return "瓶装";
    }
}
```

1. 创建实现 Item 接口的抽象类，该类提供了默认的功能。

> Burger.java

```java
public abstract class Burger implements Item {
    @Override
    public Packing packing() {
        return new Wrapper();
    }

    @Override
    public abstract float price();
}
```

> ColdDrink.java

```java
public abstract class ColdDrink implements Item{
    @Override
    public Packing packing() {
        return new Bottle();
    }

    @Override
    public abstract float price();
}
```

1. 创建扩展了 Burger 和 ColdDrink 的实体类。

> VegBurger.java

```java
public class VegBurger extends Burger{
    @Override
    public String name() {
        return "蔬菜汉堡";
    }

    @Override
    public float price() {
        return 12.5f;
    }
}
```

> ChickenBurger.java

```java
public class ChickenBurger extends Burger {
    @Override
    public String name() {
        return "鸡肉汉堡";
    }

    @Override
    public float price() {
        return 25.7f;
    }
}
```

> Sprite.java

```java
public class Sprite extends ColdDrink {
    @Override
    public String name() {
        return "雪碧";
    }

    @Override
    public float price() {
        return 3.5f;
    }
}
```

> Pepsi.java

```java
public class Pepsi extends ColdDrink {
    @Override
    public String name() {
        return "可乐";
    }

    @Override
    public float price() {
        return 3.6f;
    }
}
```

1. 创建一个 Meal 类，带有上面定义的 Item 对象。

> Meal.java

```java
import java.util.ArrayList;
import java.util.List;

public class Meal {
    public List<Item> items = new ArrayList<>();
    public void addItem(Item item){
        items.add(item);
    }
    public float getCost(){
        float cost = 0.0f;
        for (Item item : items) {
            cost += item.price();
        }
        return cost;
    }
    public void showItems(){
        for (Item item : items){
            System.out.print("食物："+item.name());
            System.out.print(",包装："+item.packing().pack());
            System.out.println(",价格："+item.price());
        }
    }
}
```

1. 创建一个 MealBuilder 类，实际的 builder 类负责创建 Meal 对象。

> MealBuilder.java

```java
public class MealBuilder {
    public Meal MealOne(){
        Meal meal = new Meal();
        meal.addItem(new VegBurger());
        meal.addItem(new Pepsi());
        return meal;
    }
    public Meal MealTwo(){
        Meal meal = new Meal();
        meal.addItem(new ChickenBurger());
        meal.addItem(new Sprite());
        return meal;
    }
}
```

1. BuiderPatternDemo 使用 MealBuider 来演示建造者模式（Builder Pattern）。

> BuilderPatternDemo.java

```java
public class BuilderPatternDemo {
    public static void main(String[] args) {
        MealBuilder mealBuilder = new MealBuilder();
        Meal mealOne = mealBuilder.MealOne();
        System.out.println("套餐一");
        mealOne.showItems();
        System.out.println("总花费："+mealOne.getCost());
        Meal mealTwo = mealBuilder.MealTwo();
        System.out.println("套餐二");
        mealTwo.showItems();
        System.out.println("总花费："+mealTwo.getCost());
    }
}
```

### 原型模式

> 目的：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。
>
> 主要解决：在运行期建立和删除原型。
>
> 应用实例：1. 一个对象需要提供给其他对象访问，而且各个调用者可能都需要修改其值时，可以考虑使用原型模式拷贝多个对象供调用者使用。2.在实际项目中，原型模式很少单独出现，一般是和工厂方法模式一起出现，通过 clone 的方法创建一个对象，然后由工厂方法提供给调用者。

1. 创建一个实现了 *Cloneable* 接口的抽象类。

> Shape.java

```java
public abstract class Shape implements Cloneable{
    private String id;
    protected String type;

    abstract void draw();

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getType() {
        return type;
    }

    @Override
    public Object clone(){
        Object clone = null;
        try {
            clone = super.clone();
        } catch (CloneNotSupportedException e){
            e.printStackTrace();
        }
        return clone;
    }
}
```

1. 创建扩展了上面抽象类的实体类。

> Circle.java

```java
public class Circle extends Shape {

    public Circle(){
        type = "圆形";
    }

    @Override
    void draw() {
        System.out.println("画一个圆形");
    }
}
```

> Rectangle.java

```java
public class Rectangle extends Shape {

    public Rectangle(){
        type = "矩形";
    }

    @Override
    void draw() {
        System.out.println("画一个矩形");
    }
}
```

> Square.java

```java
public class Square extends Shape {

    public Square(){
        type = "正方形";
    }

    @Override
    void draw() {
        System.out.println("画一个正方形");
    }
}
```

1. 创建一个类，从数据库获取实体类，并把它们存储在一个 *Hashtable* 中。

> ShapeCache.java

```java
import java.util.Hashtable;

public class ShapeCache {
    public static Hashtable<String,Shape> shapeMap = new Hashtable<>();
    public static Shape getShape(String shapeId){
        Shape cacheShape = shapeMap.get(shapeId);
        return (Shape) cacheShape.clone();
    }

    // 对每种形状都运行数据库查询，并创建该形状
    // shapeMap.put(shapeKey, shape);
    // 例如，我们要添加三种形状
    public static void loadCache(){
        Circle circle = new Circle();
        circle.setId("1");
        shapeMap.put(circle.getId(),circle);
        Rectangle rectangle = new Rectangle();
        rectangle.setId("2");
        shapeMap.put(rectangle.getId(),rectangle);
        Square square = new Square();
        square.setId("3");
        shapeMap.put(square.getId(),square);
    }
}
```

1. *PrototypePatternDemo* 使用 *ShapeCache* 类来获取存储在 *Hashtable* 中的形状的克隆。

> PrototypePatternDemo.java

```java
public class PrototypePatternDemo {
 public static void main(String[] args) {
     ShapeCache.loadCache();
     Shape circle = ShapeCache.getShape("1");
     System.out.println("形状："+circle.getType());
     Shape rectangle = ShapeCache.getShape("2");
     System.out.println("形状："+rectangle.getType());
     Shape square = ShapeCache.getShape("3");
     System.out.println("形状："+square.getType());
 }
}
```