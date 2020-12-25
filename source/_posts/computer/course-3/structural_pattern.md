---
title: 结构型模式
date: 2020/12/19 11:47
categories:
	- [计算机, 设计模式]
tags:
	- 设计模式
---

### 适配器模式

> 目的：将一个类的接口转换成客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
>
> 主要解决：主要解决在软件系统中，常常要将一些"现存的对象"放到新的环境中，而新环境要求的接口是现对象不能满足的。
>
> 应用实例：音频播放器可以播放mp3的音频文件，媒体播放器可以播放mp4和vlc格式的文件，我们想要音频播放器也可以播放mp4和vlc格式的文件，就需要适配器。

1. 为媒体播放器和更高级的媒体播放器创建接口。

>MediaPlayer.java

```java
public interface MediaPlayer {
    void play(String audioType,String fileName);
}
```

>AdvancedMediaPlayer.java

```java
public interface AdvancedMediaPlayer {
    void playVlc(String fileName);
    void playMp4(String fileName);
}
```

2. 创建实现了 *AdvancedMediaPlayer* 接口的实体类。

>Mp4Player.java

```java
public class Mp4Player implements AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {

    }

    @Override
    public void playMp4(String fileName) {
        System.out.println("播放mp4文件，文件名："+fileName);
    }
}
```

>VlcPlayer.java

```java
public class VlcPlayer implements AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {
        System.out.println("播放vlc文件，文件名："+fileName);
    }

    @Override
    public void playMp4(String fileName) {

    }
}
```

3. 创建实现了 *MediaPlayer* 接口的适配器类。

>MediaAdapter.java

```java
public class MediaAdapter implements MediaPlayer {

    AdvancedMediaPlayer advancedMediaPlayer;

    public MediaAdapter(String audioType){
        if (audioType.equalsIgnoreCase("vlc")){
            advancedMediaPlayer = new VlcPlayer();
        }else if (audioType.equalsIgnoreCase("mp4")){
            advancedMediaPlayer = new Mp4Player();
        }
    }

    @Override
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("vlc")){
            advancedMediaPlayer.playVlc(fileName);
        }else if (audioType.equalsIgnoreCase("mp4")){
            advancedMediaPlayer.playMp4(fileName);
        }
    }
}
```

4. 创建实现了 *MediaPlayer* 接口的实体类。

>AudioPlayer.java

````java
public class AudioPlayer implements MediaPlayer {

    MediaAdapter mediaAdapter;

    @Override
    public void play(String audioType, String fileName) {
        //播放 mp3 音乐文件的内置支持
        if (audioType.equalsIgnoreCase("mp3")){
            System.out.println("播放mp3文件，文件名："+fileName);
        }
        //mediaAdapter 提供了播放其他文件格式的支持
        else if (audioType.equalsIgnoreCase("vlc")){
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType,fileName);
        }else if (audioType.equalsIgnoreCase("mp4")){
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType,fileName);
        }else {
            System.out.println("不支持"+audioType+"文件");
        }
    }
}
````

5. 使用 AudioPlayer 来播放不同类型的音频格式。

>AdapterPatternDemo.java

```java
public class AdapterPatternDemo {
    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();

        audioPlayer.play("mp3","hello.mp3");
        audioPlayer.play("mp4","world.mp4");
        audioPlayer.play("vlc","hello world.vlc");
        audioPlayer.play("avi","no.avi");
    }
}
```

6. 输出结果

>播放mp3文件，文件名：hello.mp3
>播放mp4文件，文件名：world.mp4
>播放vlc文件，文件名：hello world.vlc
>不支持avi文件

### 桥接模式

> 意图：将抽象部分与实现部分分离，使它们都可以独立的变化。
>
> 主要解决：在有多种可能会变化的情况下，用继承会造成类爆炸问题，扩展起来不灵活
>
> 应用实例： 1、猪八戒从天蓬元帅转世投胎到猪，转世投胎的机制将尘世划分为两个等级，即：灵魂和肉体，前者相当于抽象化，后者相当于实现化。生灵通过功能的委派，调用肉体对象的功能，使得生灵可以动态地选择。 2、墙上的开关，可以看到的开关是抽象的，不用管里面具体怎么实现的。

1. 创建桥接实现接口。

>DrawAPI.java

```java
public interface DrawAPI {
    void drawCircle(int radius,int x,int y);
}
```

2. 创建实现了 *DrawAPI* 接口的实体桥接实现类。

> GreenCircle.java

```java
public class GreenCircle implements DrawAPI {
    @Override
    public void drawCircle(int radius, int x, int y) {
        System.out.println("画一个绿圆，半径："+radius+"，x："+x+"，y："+y);
    }
}
```

> RedCircle.java

```java
public class RedCircle implements DrawAPI {
    @Override
    public void drawCircle(int radius, int x, int y) {
        System.out.println("画一个红圆，半径："+radius+"，x："+x+"，y："+y);
    }
}
```

3. 使用 *DrawAPI* 接口创建抽象类 *Shape*。

> Shape.java

```java
public abstract class Shape {
    protected DrawAPI drawAPI;
    protected Shape(DrawAPI drawAPI){
        this.drawAPI = drawAPI;
    }
    public abstract void draw();
}
```

4. 创建实现了 *Shape* 抽象类的实体类。

> Circle.java

```java
public class Circle extends Shape {

    private int x,y,radius;

    public Circle(int x,int y,int radius,DrawAPI drawAPI){
        super(drawAPI);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }

    @Override
    public void draw() {
        drawAPI.drawCircle(radius,x,y);
    }
}
```

5. 使用 *Shape* 和 *DrawAPI* 类画出不同颜色的圆。

> BridgePatternDemo.java

```java
public class BridgePatternDemo {
    public static void main(String[] args) {
        Circle redCircle = new Circle(100, 100, 10, new RedCircle());
        Circle greenCircle = new Circle(100, 100, 10, new GreenCircle());
        redCircle.draw();
        greenCircle.draw();
    }
}
```

6. 输出结果

>画一个红圆，半径：10，x：100，y：100
>画一个绿圆，半径：10，x：100，y：100

### 过滤器模式

> 过滤器模式（Filter Pattern）或标准模式（Criteria Pattern）是一种设计模式，这种模式允许开发人员使用不同的标准来过滤一组对象，通过逻辑运算以解耦的方式把它们连接起来。这种类型的设计模式属于结构型模式，它结合多个标准来获得单一标准。

1. 创建一个类，在该类上应用标准。

> Person.java

```java
public class Person {
 private String name;
 private String gender;
 private String maritalStatus;
 public Person(String name,String gender,String maritalStatus){
     this.name = name;
     this.gender = gender;
     this.maritalStatus = maritalStatus;
 }

 public String getName() {
     return name;
 }

 public String getGender() {
     return gender;
 }

 public String getMaritalStatus() {
     return maritalStatus;
 }
}
```

2. 为标准（Criteria）创建一个接口。

> Criteria.java

```java
import java.util.List;

public interface Criteria {
    public List<Person> meetCriteria(List<Person> persons);
}
```

3. 创建实现了 *Criteria* 接口的实体类。

>CriteriaMale.java

```java
import java.util.LinkedList;
import java.util.List;

public class CriteriaMale implements Criteria {
    @Override
    public List<Person> meetCriteria(List<Person> persons) {
        List<Person> malePersons = new LinkedList<>();
        for (Person person : persons){
            if ("male".equalsIgnoreCase(person.getGender())){
                malePersons.add(person);
            }
        }
        return malePersons;
    }
}
```

>CriteriaFemale.java

```java
import java.util.ArrayList;
import java.util.List;

public class CriteriaFemale implements Criteria {
    @Override
    public List<Person> meetCriteria(List<Person> persons) {
        List<Person> femalePersons = new ArrayList<>();
        for (Person person : persons){
            if ("female".equalsIgnoreCase(person.getGender())){
                femalePersons.add(person);
            }
        }
        return femalePersons;
    }
}
```

>CriteriaSingle.java

```java
import java.util.ArrayList;
import java.util.List;

public class CriteriaSingle implements Criteria {
    @Override
    public List<Person> meetCriteria(List<Person> persons) {
        List<Person> singlePersons = new ArrayList<>();
        for (Person person : persons){
            if ("single".equalsIgnoreCase(person.getMaritalStatus())){
                singlePersons.add(person);
            }
        }
        return singlePersons;
    }
}
```

>AndCriteria.java

```java
import java.util.List;

public class AndCriteria implements Criteria {

    private Criteria criteria;
    private Criteria otherCriteria;

    public AndCriteria(Criteria criteria,Criteria otherCriteria){
        this.criteria = criteria;
        this.otherCriteria = otherCriteria;
    }

    @Override
    public List<Person> meetCriteria(List<Person> persons) {
        List<Person> firstCriteria = criteria.meetCriteria(persons);
        return otherCriteria.meetCriteria(firstCriteria);
    }
}
```

>OrCriteria.java

```java
import java.util.List;

public class OrCriteria implements Criteria{

    private Criteria criteria;
    private Criteria otherCriteria;

    public OrCriteria(Criteria criteria,Criteria otherCriteria){
        this.criteria = criteria;
        this.otherCriteria = otherCriteria;
    }

    @Override
    public List<Person> meetCriteria(List<Person> persons) {
        List<Person> firstCriteriaItems = criteria.meetCriteria(persons);
        List<Person> otherCriteriaItems = otherCriteria.meetCriteria(persons);
        for (Person person : otherCriteriaItems){
            if (!firstCriteriaItems.contains(person)){
                firstCriteriaItems.add(person);
            }
        }
        return firstCriteriaItems;
    }
}
```

4. 使用不同的标准（Criteria）和它们的结合来过滤 *Person* 对象的列表。

>CriteriaPatternDemo.java

```java
import java.util.ArrayList;
import java.util.List;

public class CriteriaPatternDemo {
    public static void main(String[] args) {
        List<Person> persons = new ArrayList<>();
        persons.add(new Person("张三","Male", "Single"));
        persons.add(new Person("李四","Male", "Married"));
        persons.add(new Person("王五","Female", "Married"));
        persons.add(new Person("赵六","Female", "Single"));
        persons.add(new Person("赵七","Male", "Single"));
        persons.add(new Person("赵八","Male", "Single"));

        Criteria male = new CriteriaMale();
        Criteria female = new CriteriaFemale();
        Criteria single = new CriteriaSingle();
        Criteria singleMale = new AndCriteria(single, male);
        Criteria singleOrFemale = new OrCriteria(single, female);

        System.out.println("男性: ");
        printPersons(male.meetCriteria(persons));

        System.out.println("女性: ");
        printPersons(female.meetCriteria(persons));

        System.out.println("单身男性: ");
        printPersons(singleMale.meetCriteria(persons));

        System.out.println("单身或者女性: ");
        printPersons(singleOrFemale.meetCriteria(persons));
    }

    public static void printPersons(List<Person> persons){
        for (Person person : persons) {
            System.out.println("Person : [ Name : " + person.getName()
                               +", Gender : " + person.getGender()
                               +", Marital Status : " + person.getMaritalStatus()
                               +" ]");
        }
    }
}
```

5. 输出结果

>男性: 
>Person : [ Name : 张三, Gender : Male, Marital Status : Single ]
>Person : [ Name : 李四, Gender : Male, Marital Status : Married ]
>Person : [ Name : 赵七, Gender : Male, Marital Status : Single ]
>Person : [ Name : 赵八, Gender : Male, Marital Status : Single ]
>女性: 
>Person : [ Name : 王五, Gender : Female, Marital Status : Married ]
>Person : [ Name : 赵六, Gender : Female, Marital Status : Single ]
>单身女性: 
>Person : [ Name : 张三, Gender : Male, Marital Status : Single ]
>Person : [ Name : 赵七, Gender : Male, Marital Status : Single ]
>Person : [ Name : 赵八, Gender : Male, Marital Status : Single ]
>单身或者女性: 
>Person : [ Name : 张三, Gender : Male, Marital Status : Single ]
>Person : [ Name : 赵六, Gender : Female, Marital Status : Single ]
>Person : [ Name : 赵七, Gender : Male, Marital Status : Single ]
>Person : [ Name : 赵八, Gender : Male, Marital Status : Single ]
>Person : [ Name : 王五, Gender : Female, Marital Status : Married ]

### 组合模式

> 目的：将对象组合成树形结构以表示"部分-整体"的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。
>
> 主要解决：它在我们树型结构的问题中，模糊了简单元素和复杂元素的概念，客户程序可以像处理简单元素一样来处理复杂元素，从而使得客户程序与复杂元素的内部结构解耦。
>
> 应用实例： 算术表达式包括操作数、操作符和另一个操作数，其中，另一个操作符也可以是操作数、操作符和另一个操作数。
>
> 使用场景：部分、整体场景，如树形菜单，文件、文件夹的管理。

1. 创建 *Employee* 类，该类带有 *Employee* 对象的列表。

>Employee.java

```java
import java.util.ArrayList;
import java.util.List;

public class Employee {
    private String name;
    private String dept;
    private int salary;
    private List<Employee> subordinates;

    public Employee(String name,String dept,int salary){
        this.name = name;
        this.dept = dept;
        this.salary = salary;
        subordinates = new ArrayList<>();
    }

    public void add(Employee e){
        subordinates.add(e);
    }

    public void remove(Employee e){
        subordinates.remove(e);
    }

    public List<Employee> getSubordinates(){
        return subordinates;
    }

    @Override
    public String toString() {
        return "Employee{" +
            "name='" + name + '\'' +
            ", dept='" + dept + '\'' +
            ", salary=" + salary +
            '}';
    }
}
```

2. 使用 *Employee* 类来创建和打印员工的层次结构。

> CompositePatternDemo.java

```java
public class CompositePatternDemo {
    public static void main(String[] args) {
        Employee ceo = new Employee("张三","CEO",30000);
        Employee headSales = new Employee("李四","销售部主管",20000);
        Employee headPersons = new Employee("王五","人事部主管",20000);
        Employee sale1 = new Employee("赵六","销售职工",10000);
        Employee sale2 = new Employee("赵七","销售职工",10000);
        Employee person1 = new Employee("赵八","人事职工",10000);
        Employee person2 = new Employee("赵九","人事职工",10000);

        ceo.add(headSales);
        ceo.add(headPersons);

        headSales.add(sale1);
        headSales.add(sale2);

        headPersons.add(person1);
        headPersons.add(person2);

        System.out.println(ceo);
        for (Employee headEmployee : ceo.getSubordinates()){
            System.out.println(headEmployee);
            for (Employee employee : headEmployee.getSubordinates()){
                System.out.println(employee);
            }
        }
    }
}
```

3. 输出结果

>Employee{name='张三', dept='CEO', salary=30000}
>Employee{name='李四', dept='销售部主管', salary=20000}
>Employee{name='赵六', dept='销售职工', salary=10000}
>Employee{name='赵七', dept='销售职工', salary=10000}
>Employee{name='王五', dept='人事部主管', salary=20000}
>Employee{name='赵八', dept='人事职工', salary=10000}
>Employee{name='赵九', dept='人事职工', salary=10000}

### 装饰器模式

>意图：动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。
>
>主要解决：一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。
>
>应用实例：不论一幅画有没有画框都可以挂在墙上，但是通常都是有画框的，并且实际上是画框被挂在墙上。在挂在墙上之前，画可以被蒙上玻璃，装到框子里；这时画、玻璃和画框形成了一个物体。
>
>使用场景： 1、扩展一个类的功能。 2、动态增加功能，动态撤销。

1. 创建一个接口

>Shape.java

```java
public interface Shape {
    void draw();
}
```

2. 创建实现接口的实体类。

> Circle.java

```java
public class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("圆形");
    }
}
```

> Rectangle.java

```java
public class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("正方形");
    }
}
```

3. 创建实现了 *Shape* 接口的抽象装饰类。

> ShapeDecorator.java

```java
public abstract class ShapeDecorator implements Shape {

    protected Shape decoratorShape;

    public ShapeDecorator(Shape decoratorShape){
        this.decoratorShape = decoratorShape;
    }

    @Override
    public void draw() {
        decoratorShape.draw();
    }
}
```

4. 创建扩展了 *ShapeDecorator* 类的实体装饰类。

> RedShapeDecorator.java

```java
public class RedShapeDecorator extends ShapeDecorator {

    public RedShapeDecorator(Shape decoratorShape){
        super(decoratorShape);
    }

    @Override
    public void draw() {
        decoratorShape.draw();
        setRedBorder(decoratorShape);
    }

    public void setRedBorder(Shape decoratorShape){
        System.out.println("红色边框");
    }
}
```

5. 使用 *RedShapeDecorator* 来装饰 *Shape* 对象。

> DecoratorPatternDemo.java

```java
public class DecoratorPatternDemo {
    public static void main(String[] args) {
        Shape circle = new Circle();
        ShapeDecorator redCircle = new RedShapeDecorator(new Circle());
        ShapeDecorator redRectangle = new RedShapeDecorator(new Rectangle());
        System.out.println("正常边框的圆形：");
        circle.draw();
        System.out.println("红色边框的圆形：");
        redCircle.draw();
        System.out.println("红色边框的矩形：");
        redRectangle.draw();
    }
}
```

6. 输出结果

> 正常边框的圆形：
> 圆形
> 红色边框的圆形：
> 圆形
> 红色边框
> 红色边框的矩形：
> 正方形
> 红色边框

### 外观模式

> 目的：为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。
>
> 主要解决：降低访问复杂系统的内部子系统时的复杂度，简化客户端与之的接口。
>
> 应用实例： 1、去医院看病，可能要去挂号、门诊、划价、取药，让患者或患者家属觉得很复杂，如果有提供接待人员，只让接待人员来处理，就很方便。 2、JAVA 的三层开发模式。
>
> 使用场景： 1、为复杂的模块或子系统提供外界访问的模块。 2、子系统相对独立。 3、预防低水平人员带来的风险。

1. 创建一个接口。

>Shape.java

````java
public interface Shape {
    void draw();
}
````

2. 创建实现接口的实体类。

> Circle.java

```java
public class Circle implements Shape{
    @Override
    public void draw() {
        System.out.println("画一个圆形");
    }
}
```

> Rectangle.java

```java
public class Rectangle implements Shape{
    @Override
    public void draw() {
        System.out.println("画一个矩形");
    }
}
```

> Square.java

```java
public class Square implements Shape {
    @Override
    public void draw() {
        System.out.println("画一个正方形");
    }
}
```

3. 创建一个外观类。

> ShapeMaker.java

```java
public class ShapeMaker {
    private Shape circle;
    private Shape rectangle;
    private Shape square;

    public ShapeMaker(){
        circle = new Circle();
        rectangle = new Rectangle();
        square = new Square();
    }

    public void drawCircle(){
        circle.draw();
    }
    public void drawRectangle(){
        rectangle.draw();
    }
    public void drawSquare(){
        square.draw();
    }
}
```

4. 使用该外观类画出各种类型的形状。

> FacadePatternDemo.java

```java
public class FacadePatternDemo {
    public static void main(String[] args) {
        ShapeMaker shapeMaker = new ShapeMaker();
        shapeMaker.drawCircle();
        shapeMaker.drawRectangle();
        shapeMaker.drawSquare();
    }
}
```

5. 输出结果

>画一个圆形
>
>画一个矩形
>
>画一个正方形

### 享元模式

> 目的：运用共享技术有效地支持大量细粒度的对象。
>
> 主要解决：在有大量对象时，有可能会造成内存溢出，我们把其中共同的部分抽象出来，如果有相同的业务请求，直接返回在内存中已有的对象，避免重新创建。
>
> 应用实例： 1、JAVA 中的 String，如果有则返回，如果没有则创建一个字符串保存在字符串缓存池里面。 2、数据库的数据池。
>
> 使用场景： 1、系统有大量相似对象。 2、需要缓冲池的场景。

1. 创建一个接口。

> Shape.java

```java
public interface Shape {
    void draw();
}
```

2. 创建实现接口的实体类。

> Circle.java

```java
public class Circle implements Shape{

    private String color;
    private int x;
    private int y;
    private int radius;

    public Circle(String color){
        this.color = color;
    }
    public void setX(int x){
        this.x = x;
    }
    public void setY(int y){
        this.y = y;
    }

    @Override
    public void draw() {
        System.out.println("画一个圆，颜色："+color+"，x："+x+"，y："+y+"，raduis："+radius);
    }
}
```

3. 创建一个工厂，生成基于给定信息的实体类的对象。

>ShapeFactory.java

````java
import java.util.HashMap;

public class ShapeFactory {
    private static final HashMap<String,Shape> circleMap = new HashMap<>();

    public static Shape getCircle(String color){
        Circle circle = (Circle) circleMap.get(color);

        if (circle == null){
            circle = new Circle(color);
            circleMap.put(color,circle);
            System.out.println("创建一个有颜色的圆形："+color);
        }
        return circle;
    }
}
````

4. 使用该工厂，通过传递颜色信息来获取实体类的对象。

> FlyweightPatternDemo.java

```java
public class FlyweightPatternDemo {

    private static final String[] colors = {"红色","黄色","绿色","蓝色","白色"};

    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            Circle circle = (Circle) ShapeFactory.getCircle(getRandomColor());
            circle.setX(getRandomX());
            circle.setY(getRandomY());
            circle.setRadius(100);
            circle.draw();
        }
    }

    private static int getRandomY() {
        return (int) (Math.random()*100);
    }

    private static int getRandomX() {
        return (int) (Math.random()*100);
    }

    private static String getRandomColor() {
        return colors[(int) (Math.random()*colors.length)];
    }
}
```

5. 输出结果

>创建一个有颜色的圆形：白色
>
>画一个圆，颜色：白色，x：44，y：48，raduis：100
>
>画一个圆，颜色：绿色，x：83，y：14，raduis：100
>
>创建一个有颜色的圆形：红色
>
>画一个圆，颜色：红色，x：95，y：76，raduis：100
>
>画一个圆，颜色：绿色，x：45，y：4，raduis：100
>
>画一个圆，颜色：绿色，x：37，y：9，raduis：100
>
>画一个圆，颜色：红色，x：6，y：70，raduis：100
>
>创建一个有颜色的圆形：蓝色
>
>画一个圆，颜色：蓝色，x：44，y：30，raduis：100
>
>创建一个有颜色的圆形：黄色
>
>画一个圆，颜色：黄色，x：95，y：49，raduis：100
>
>画一个圆，颜色：红色，x：65，y：4，raduis：100

### 代理模式

> 意图：为其他对象提供一种代理以控制对这个对象的访问。
>
> 主要解决：在直接访问对象时带来的问题，比如说：要访问的对象在远程的机器上。在面向对象系统中，有些对象由于某些原因（比如对象创建开销很大，或者某些操作需要安全控制，或者需要进程外的访问），直接访问会给使用者或者系统结构带来很多麻烦，我们可以在访问此对象时加上一个对此对象的访问层。
>
> 应用实例： 1、Windows 里面的快捷方式。2、买火车票不一定在火车站买，也可以去代售点。3、spring aop。
>
> 使用场景：按职责来划分，通常有以下使用场景： 1、远程代理。 2、虚拟代理。 3、Copy-on-Write 代理。 4、保护（Protect or Access）代理。 5、Cache代理。 6、防火墙（Firewall）代理。 7、同步化（Synchronization）代理。 8、智能引用（Smart Reference）代理。

1. 创建一个接口。

> Image.java

```java
public interface Image {
    void display();
}
```

2. 创建实现接口的实体类。

> RealImage.java

```java
public class RealImage implements Image {

    private String fileName;

    public RealImage(String fileName){
        this.fileName = fileName;
        loadFromDisk(fileName);
    }

    private void loadFromDisk(String fileName) {
        System.out.println("磁盘加载"+fileName);
    }

    @Override
    public void display() {
        System.out.println("显示"+fileName);
    }
}
```

> ProxyImage.java

````java
public class ProxyImage implements Image {
    private RealImage realImage;
    private String fileName;

    public ProxyImage(String fileName){
        this.fileName = fileName;
    }

    @Override
    public void display() {
        if (realImage == null){
            realImage = new RealImage(fileName);
        }
        realImage.display();
    }
}
````

3. 当被请求时，使用 *ProxyImage* 来获取 *RealImage* 类的对象。

> ProxyPatternDemo.java

```java
public class ProxyPatternDemo {
    public static void main(String[] args) {
        ProxyImage proxyImage = new ProxyImage("test.jpg");
        //从磁盘加载
        proxyImage.display();
        //不从磁盘加载
        proxyImage.display();
    }
}
```

4.输出结果

> 磁盘加载test.jpg
> 显示test.jpg
> 显示test.jpg
