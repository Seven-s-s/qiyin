---
title: 行为型模式
date: 2020/12/19 11:48
categories:
	- [计算机, 设计模式]
tags: 设计模式

---

### 责任链模式

> 目的：避免请求发送者与接收者耦合在一起，让多个对象都有可能接收请求，将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止。
>
> 主要解决：职责链上的处理者负责处理请求，客户只需要将请求发送到职责链上即可，无须关心请求的处理细节和请求的传递，所以职责链将请求的发送者和请求的处理者解耦了。
>
> 应用实例： 1、红楼梦中的"击鼓传花"。 2、JS 中的事件冒泡。 3、JAVA WEB 中 Apache Tomcat 对 Encoding 的处理，Struts2 的拦截器，jsp servlet 的 Filter。
>
> 使用场景： 1、有多个对象可以处理同一个请求，具体哪个对象处理该请求由运行时刻自动确定。 2、在不明确指定接收者的情况下，向多个对象中的一个提交一个请求。 3、可动态指定一组对象处理请求。

1. 创建抽象的记录器类。

> AbstractLogger.java

```java
public abstract class AbstractLogger {
    public static int INFO = 1;
    public static int DEBUG = 2;
    public static int ERROR = 3;

    protected int level;

    //责任链的下一个元素
    protected AbstractLogger nextLogger;

    public void setNextLogger(AbstractLogger nextLogger){
        this.nextLogger = nextLogger;
    }

    public void logMessage(int level,String message){
        if (this.level <= level){
            write(message);
        }
        if (nextLogger != null){
            nextLogger.logMessage(level,message);
        }
    }

    protected abstract void write(String message);

}
```

2. 创建扩展了该记录器类的实体类。

>ConsoleLogger.java

```java
public class ConsoleLogger extends AbstractLogger {

    public ConsoleLogger(int level){
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("标准打印::日志:"+message);
    }
}
```

> FileLogger.java

```java
public class FileLogger extends AbstractLogger {

    public FileLogger(int level){
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("文件打印:日志:"+message);
    }
}
```

> ErrorLogger.java

```java
public class ErrorLogger extends AbstractLogger {

    public ErrorLogger(int level){
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("错误打印::日志:"+message);
    }
}
```

3. 创建不同类型的记录器。赋予它们不同的错误级别，并在每个记录器中设置下一个记录器。每个记录器中的下一个记录器代表的是链的一部分。

> ChainPatternDemo.java

```java
public class ChainPatternDemo {
    public static void main(String[] args) {
        AbstractLogger loggerChain = getChainOfLoggers();
        loggerChain.logMessage(AbstractLogger.INFO,"这是一个信息");
        System.out.println();
        loggerChain.logMessage(AbstractLogger.DEBUG,"这是一个调试信息");
        System.out.println();
        loggerChain.logMessage(AbstractLogger.ERROR,"这是一个错误信息");
    }
    public static AbstractLogger getChainOfLoggers(){
        AbstractLogger errorLogger = new ErrorLogger(AbstractLogger.ERROR);
        FileLogger fileLogger = new FileLogger(AbstractLogger.DEBUG);
        ConsoleLogger consoleLogger = new ConsoleLogger(AbstractLogger.INFO);

        errorLogger.setNextLogger(fileLogger);
        fileLogger.setNextLogger(consoleLogger);

        return errorLogger;
    }
}
```

4. 输出结果

> 标准打印::日志:这是一个信息
>
> 文件打印:日志:这是一个调试信息
>
> 标准打印::日志:这是一个调试信息
>
> 错误打印::日志:这是一个错误信息
>
> 文件打印:日志:这是一个错误信息
>
> 标准打印::日志:这是一个错误信息

### 命令模式

> 意图：将一个请求封装成一个对象，从而使您可以用不同的请求对客户进行参数化。
>
> 主要解决：在软件系统中，行为请求者与行为实现者通常是一种紧耦合的关系，但某些场合，比如需要对行为进行记录、撤销或重做、事务等处理时，这种无法抵御变化的紧耦合的设计就不太合适。
>
> 应用实例：struts 1 中的 action 核心控制器 ActionServlet 只有一个，相当于 Invoker，而模型层的类会随着不同的应用有不同的模型类，相当于具体的 Command。
>
> 使用场景：认为是命令的地方都可以使用命令模式，比如： 1、GUI 中每一个按钮都是一条命令。 2、模拟 CMD。

1. 创建一个命令接口。

> Order.java

```java
public interface Order {
    void excute();
}
```

2. 创建一个请求类。

> Stock.java

```java
public class Stock {
    private String name = "苹果";
    private int quantity = 10;

    public void buy(){
        System.out.println("购买商品[名字："+name+"，数量："+quantity+"]");
    }

    public void sell(){
        System.out.println("销售商品[名字："+name+"，数量："+quantity+"]");
    }
}
```

3. 创建实现了 *Order* 接口的实体类。

> BuyStock.java

```java
public class BuyStock implements Order {

    private Stock appleStock;

    public BuyStock(Stock appleStock){
        this.appleStock = appleStock;
    }

    @Override
    public void excute() {
        appleStock.buy();
    }
}
```

> SellStock.java

```java
public class SellStock implements Order {

    private Stock appleStock;

    public SellStock(Stock appleStock){
        this.appleStock = appleStock;
    }

    @Override
    public void excute() {
        appleStock.sell();
    }
}
```

4. 创建命令调用类。

> Broker.java

```java
import java.util.ArrayList;
import java.util.List;

public class Broker {
    private List<Order> orderList = new ArrayList<>();

    public void takeOrder(Order order){
        orderList.add(order);
    }

    public void plcaeOrder(){
        for (Order order : orderList){
            order.excute();
        }
        orderList.clear();
    }
}
```

5. 使用 Broker 类来接受并执行命令。

> CommandPatternDemo.java

```java
public class CommandPatternDemo {
    public static void main(String[] args) {
        Stock appleStock = new Stock();

        BuyStock buyStockOrder = new BuyStock(appleStock);
        SellStock sellStockOrder = new SellStock(appleStock);

        Broker broker = new Broker();
        broker.takeOrder(buyStockOrder);
        broker.takeOrder(sellStockOrder);

        broker.plcaeOrder();
    }
}
```

6. 输出结果

> 购买商品[名字：苹果，数量：10]
>
> 销售商品[名字：苹果，数量：10]

### 解释器模式

>意图：给定一个语言，定义它的文法表示，并定义一个解释器，这个解释器使用该标识来解释语言中的句子。
>
>主要解决：对于一些固定文法构建一个解释句子的解释器。
>
>应用实例：编译器、运算表达式计算。
>
>使用场景： 1、可以将一个需要解释执行的语言中的句子表示为一个抽象语法树。 2、一些重复出现的问题可以用一种简单的语言来进行表达。 3、一个简单语法需要解释的场景。

1. 创建一个表达式接口。

> Expression.java

 ```java
public interface Expression {
     boolean interpret(String context);
}
 ```

2. 创建实现了上述接口的实体类。

> TerminalExpression.java

 ```java
public class TerminalExpression implements Expression {

    private String data;

    public TerminalExpression(String data){
        this.data = data;
    }

    @Override
    public boolean interpret(String context) {
        if (context.contains(data)){
            return true;
        }
        return false;
    }
}
 ```

> AndExpression.java

````java
public class AndExpression implements Expression {

    private Expression expr1 = null;
    private Expression expr2 = null;

    public AndExpression(Expression expr1,Expression expr2){
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    @Override
    public boolean interpret(String context) {
        return expr1.interpret(context) && expr2.interpret(context);
    }
}
````

> OrExpression.java

```java
public class OrExpression implements Expression {

    private Expression expr1 = null;
    private Expression expr2 = null;

    public OrExpression(Expression expr1, Expression expr2){
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    @Override
    public boolean interpret(String context) {
        return expr1.interpret(context) || expr2.interpret(context);
    }
}
```

3. InterpreterPatternDemo使用 *Expression* 类来创建规则，并解析它们。

> InterpreterPatternDemo.java

```java
public class InterpreterPatternDemo {

    //规则：张三 和 李四 是男性
    public static Expression getMaleExpression(){
        Expression zhangsan = new TerminalExpression("张三");
        Expression lisi = new TerminalExpression("李四");
        return new OrExpression(zhangsan,lisi);
    }

    //规则：王五是一个已婚的女性
    public static Expression getMarriedWomenExpression(){
        Expression wangwu = new TerminalExpression("王五");
        Expression married = new TerminalExpression("已结婚");
        return new AndExpression(wangwu,married);
    }

    public static void main(String[] args) {
        Expression isMale = getMaleExpression();
        Expression isMarriedWomen = getMarriedWomenExpression();

        System.out.println("张三是男性？"+isMale.interpret("张三"));
        System.out.println("王五是一个已婚的女性？"+isMarriedWomen.interpret("王五已结婚"));
    }
}
```

4. 输出结果

> 张三是男性？true
>
> 王五是一个已婚的女性？true

### 迭代器模式

> 意图：提供一种方法顺序访问一个聚合对象中各个元素, 而又无须暴露该对象的内部表示。
>
> 主要解决：不同的方式来遍历整个整合对象。
>
> 应用实例：JAVA 中的 iterator。
>
> 使用场景： 1、访问一个聚合对象的内容而无须暴露它的内部表示。 2、需要为聚合对象提供多种遍历方式。 3、为遍历不同的聚合结构提供一个统一的接口。

1. 创建接口。

> Iterator.java

```java
public interface Iterator {
    boolean hasNext();
    Object next();
}
```

2. 创建实现了 *Container* 接口的实体类。该类有实现了 *Iterator* 接口的内部类 *NameIterator*。

> Container.java

```java
public interface Container {
    Iterator getIterator();
}
```

> NameRepository.java

```java
public class NameRepository implements Container{
    public String[] names = {"张三","李四","王五","赵六"};

    @Override
    public Iterator getIterator() {
        return new NameInterator();
    }
    private class NameInterator implements Iterator{

        int index;

        @Override
        public boolean hasNext() {
            if (index < names.length){
                return true;
            }
            return false;
        }

        @Override
        public Object next() {
            if (this.hasNext()){
                return names[index++];
            }
            return null;
        }
    }
}
```

3. 使用 *NameRepository* 来获取迭代器，并打印名字。

> IteratorPatternDemo.java

```java
public class IteratorPatternDemo {
    public static void main(String[] args) {
        NameRepository nameRepository = new NameRepository();

        for (Iterator iterator = nameRepository.getIterator();iterator.hasNext();){
            String name = (String) iterator.next();
            System.out.println(name);
        }
    }
}
```

4. 输出结果

> 张三
>
> 李四
>
> 王五
>
> 赵六

### 中介者模式

> 意图：用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。
>
> 主要解决：对象与对象之间存在大量的关联关系，这样势必会导致系统的结构变得很复杂，同时若一个对象发生改变，我们也需要跟踪与之相关联的对象，同时做出相应的处理。
>
> 应用实例： 1、中国加入 WTO 之前是各个国家相互贸易，结构复杂，现在是各个国家通过 WTO 来互相贸易。 2、机场调度系统。 3、MVC 框架，其中C（控制器）就是 M（模型）和 V（视图）的中介者。
>
> 使用场景： 1、系统中对象之间存在比较复杂的引用关系，导致它们之间的依赖关系结构混乱而且难以复用该对象。 2、想通过一个中间类来封装多个类中的行为，而又不想生成太多的子类。

1. 创建中介类。

> ChatRoom.java

```java
public class ChatRoom {
    public static void showMessage(User user,String message){
        System.out.println("["+user.getName()+"]"+message);
    }
}
```

2. 创建 user 类。

> User.java

```java
public class User {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    public User(String name){
        this.name = name;
    }
    public void sendMessage(String message){
        ChatRoom.showMessage(this,message);
    }
}
```

3. 使用 *User* 对象来显示他们之间的通信。

> MediatorPatternDemo.java

```java
public class MediatorPatternDemo {
    public static void main(String[] args) {
        User zhangsan = new User("张三");
        User lisi = new User("李四");

        zhangsan.sendMessage("你好！李四");
        lisi.sendMessage("你好！张三");
    }
}
```

4. 输出结果

> [张三]你好！李四
>
> [李四]你好！张三

### 备忘录模式

> 意图：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。
>
> 主要解决：所谓备忘录模式就是在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态。
>
> 应用实例： 1、后悔药。 2、打游戏时的存档。 3、Windows 里的 ctri + z。 4、IE 中的后退。 4、数据库的事务管理。
>
> 使用场景： 1、需要保存/恢复数据的相关状态场景。 2、提供一个可回滚的操作。

1. 创建 Memento 类。

> Memento.java

```java
public class Memento {
    private String state;

    public Memento(String state){
        this.state = state;
    }
    public String getState(){
        return state;
    }
}
```

2. 创建 Originator 类。

> Originator.java

```java
public class Originator {
    private String state;

    public void setState(String state){
        this.state = state;
    }

    public String getState(){
        return state;
    }

    public Memento saveStateToMemento(){
        return new Memento(state);
    }

    public void getStateFromMemento(Memento memento){
        state = memento.getState();
    }
}
```

3. 创建 CareTaker 类。

> CareTaker.java

```java
import java.util.ArrayList;
import java.util.List;

public class CareTaker {
    private List<Memento> mementoList = new ArrayList<>();

    public void add(Memento state){
        mementoList.add(state);
    }

    public Memento get(int index){
        return mementoList.get(index);
    }
}
```

4. 使用 *CareTaker* 和 *Originator* 对象。

> MementoPatternDemo.java

```java
public class MementoPatternDemo {
    public static void main(String[] args) {
        Originator originator = new Originator();
        CareTaker careTaker = new CareTaker();
        originator.setState("状态 #1");
        originator.setState("状态 #2");
        careTaker.add(originator.saveStateToMemento());
        originator.setState("状态 #3");
        careTaker.add(originator.saveStateToMemento());
        originator.setState("状态 #4");
        System.out.println("当前状态："+originator.getState());
        originator.getStateFromMemento(careTaker.get(0));
        System.out.println("第一次保存的状态："+originator.getState());
        originator.getStateFromMemento(careTaker.get(1));
        System.out.println("第二次保存的状态："+originator.getState());
    }
}
```

5. 输出结果

> 当前状态：状态 #4
>
> 第一次保存的状态：状态 #2
>
> 第二次保存的状态：状态 #3

### 观察者模式

> 意图：定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。
>
> 主要解决：一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。
>
> 应用实例： 拍卖的时候，拍卖师观察最高标价，然后通知给其他竞价者竞价。
>
> 使用场景：1、一个抽象模型有两个方面，其中一个方面依赖于另一个方面。将这些方面封装在独立的对象中使它们可以各自独立地改变和复用。2、一个对象的改变将导致其他一个或多个对象也发生改变，而不知道具体有多少对象将发生改变，可以降低对象之间的耦合度。3、一个对象必须通知其他对象，而并不知道这些对象是谁。4、需要在系统中创建一个触发链，A对象的行为将影响B对象，B对象的行为将影响C对象……，可以使用观察者模式创建一种链式触发机制。

1. 创建 Subject 类。

> Subject.java

```java
import java.util.ArrayList;
import java.util.List;

public class Subject {
    private List<Observer> observers = new ArrayList<>();
    private int state;

    public int getState() {
        return state;
    }

    public void setState(int state) {
        this.state = state;
        notifyAllObserver();
    }

    public void attach(Observer observer){
        observers.add(observer);
    }

    private void notifyAllObserver() {
        for (Observer observer : observers){
            observer.update();
        }
    }

}
```

2. 创建 Observer 类。

> Observer.java

```java
public abstract class Observer {
    protected Subject subject;
    public abstract void update();
}
```

3. 创建实体观察者类。

> BinaryObserver.java

```java
public class BinaryObserver extends Observer {

    public BinaryObserver(Subject subject){
        this.subject = subject;
        this.subject.attach(this);
    }

    @Override
    public void update() {
        System.out.println("二进制："+Integer.toBinaryString(subject.getState()));
    }
}
```

> OctalObserver.java

```java
public class OctalObserver extends Observer {

    public OctalObserver(Subject subject){
        this.subject = subject;
        this.subject.attach(this);
    }

    @Override
    public void update() {
        System.out.println("八进制："+Integer.toOctalString(subject.getState()));
    }
}
```

> HexaObserver.java

```java
public class HexaObserver extends Observer {

    public HexaObserver(Subject subject){
        this.subject = subject;
        this.subject.attach(this);
    }

    @Override
    public void update() {
        System.out.println("十六进制："+Integer.toHexString(subject.getState()));
    }
}
```

4. 使用 *Subject* 和实体观察者对象。

> ObserverPatternDemo.java

```java
public class ObserverPatternDemo {
    public static void main(String[] args) {
        Subject subject = new Subject();
        new BinaryObserver(subject);
        new OctalObserver(subject);
        new HexaObserver(subject);

        System.out.println("第一次状态改为15");
        subject.setState(15);
        System.out.println("第二次状态改为10");
        subject.setState(10);
    }
}
```

5. 输出结果

> 第一次状态改为15
>
> 二进制：1111
>
> 八进制：17
>
> 十六进制：f
>
> 第二次状态改为10
>
> 二进制：1010
>
> 八进制：12
>
> 十六进制：a

### 状态模式

> 意图：允许对象在内部状态发生改变时改变它的行为，对象看起来好像修改了它的类。
>
> 主要解决：对象的行为依赖于它的状态（属性），并且可以根据它的状态改变而改变它的相关行为。
>
> 应用实例： 打篮球的时候运动员可以有正常状态、不正常状态和超常状态。
>
> 使用场景： 1、行为随状态改变而改变的场景。 2、条件、分支语句的代替者。

1. 创建一个接口。

> State.java

```java
public interface State {
    void doAction(Context context);
}
```

2. 创建实现接口的实体类。

> StartState.java

```java
public class StartState implements State {
    @Override
    public void doAction(Context context) {
        System.out.println("玩家处于一个开始状态");
        context.setState(this);
    }

    @Override
    public String toString(){
        return "开始状态";
    }
}
```

> StopState.java

```java
public class StopState implements State {
    @Override
    public void doAction(Context context) {
        System.out.println("玩家处于一个停止状态");
        context.setState(this);
    }

    @Override
    public String toString() {
        return "停止状态";
    }
}
```

3. 创建 *Context* 类。

> Context.java

````java
public class Context {
    private State state;

    public Context(){
        state = null;
    }

    public void setState(State state){
        this.state = state;
    }

    public State getState() {
        return state;
    }
}
````

4. 使用 *Context* 来查看当状态 *State* 改变时的行为变化。

> StatePatternDemo.java

```java
public class StatePatternDemo {
    public static void main(String[] args) {
        Context context = new Context();

        StartState startState = new StartState();
        startState.doAction(context);
        System.out.println(context.getState().toString());

        StopState stopState = new StopState();
        stopState.doAction(context);
        System.out.println(context.getState().toString());
    }
}
```

5. 输出结果

> 玩家处于一个开始状态
> 开始状态
> 玩家处于一个停止状态
> 停止状态

### 策略模式

> 意图：定义一系列的算法,把它们一个个封装起来, 并且使它们可相互替换。
>
> 主要解决：在有多种算法相似的情况下，使用 if...else 所带来的复杂和难以维护。
>
> 应用实例： 1、诸葛亮的锦囊妙计，每一个锦囊就是一个策略。 2、旅行的出游方式，选择骑自行车、坐汽车，每一种旅行方式都是一个策略。 3、JAVA AWT 中的 LayoutManager。
>
> 使用场景： 1、如果在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。 2、一个系统需要动态地在几种算法中选择一种。 3、如果一个对象有很多的行为，如果不用恰当的模式，这些行为就只好使用多重的条件选择语句来实现。

1. 创建一个接口。

> Strategy.java

```java
public interface Strategy {
    int doOperation(int num1,int num2);
}
```

2. 创建实现接口的实体类。

> OperationAdd.java

```java
public class OperationAdd implements Strategy {
    @Override
    public int doOperation(int num1, int num2) {
        return num1 + num2;
    }
}
```

> OperationSubtract.java

```java
public class OperationSubtract implements Strategy {
    @Override
    public int doOperation(int num1, int num2) {
        return num1 - num2;
    }
}
```

> OperationMultiply.java

````java
public class OperationMultiply implements Strategy {
    @Override
    public int doOperation(int num1, int num2) {
        return num1 * num2;
    }
}
````

3. 创建 *Context* 类。

> Context.java

```java
public class Context {
    private Strategy strategy;

    public Context(Strategy strategy) {
        this.strategy = strategy;
    }

    public int executeStrategy(int num1, int num2){
        return strategy.doOperation(num1,num2);
    }
}
```

4. 使用 *Context* 来查看当它改变策略 *Strategy* 时的行为变化。

> StrategyPatternDemo.java

```java
public class StrategyPatternDemo {
    public static void main(String[] args) {
        Context context = new Context(new OperationAdd());
        System.out.println("10 + 5 = "+context.executeStrategy(10,5));

        context = new Context(new OperationSubtract());
        System.out.println("10 - 5 = "+context.executeStrategy(10,5));

        context = new Context(new OperationMultiply());
        System.out.println("10 * 5 = "+context.executeStrategy(10,5));
    }
}
```

5. 输出结果

> 10 + 5 = 15
>
> 10 - 5 = 5
>
> 10 * 5 = 50

### 模板方法

> 意图：定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。
>
> 主要解决：一些方法通用，却在每一个子类都重新写了这一方法。
>
> 应用实例： 1、在造房子的时候，地基、走线、水管都一样，只有在建筑的后期才有加壁橱加栅栏等差异。 2、西游记里面菩萨定好的 81 难，这就是一个顶层的逻辑骨架。 3、spring 中对 Hibernate 的支持，将一些已经定好的方法封装起来，比如开启事务、获取 Session、关闭 Session 等，程序员不重复写那些已经规范好的代码，直接丢一个实体就可以保存。
>
> 使用场景： 1、有多个子类共有的方法，且逻辑相同。 2、重要的、复杂的方法，可以考虑作为模板方法。

1. 创建一个抽象类，它的模板方法被设置为 final。

> Game.java

```java
public abstract class Game {
    abstract void initialize();
    abstract void startPlay();
    abstract void endPlay();

    //模板
    public final void play(){
        //初始化游戏
        initialize();
        //开始游戏
        startPlay();
        //结束游戏
        endPlay();
    }
}
```

2. 创建扩展了上述类的实体类。

> Cricket.java

```java
public class Cricket extends Game {
    @Override
    void initialize() {
        System.out.println("板球游戏初始化完成，请开始游戏");
    }

    @Override
    void startPlay() {
        System.out.println("板球游戏已开始");
    }

    @Override
    void endPlay() {
        System.out.println("板球游戏已结束");
    }
}
```

> Football.java

```java
public class Football extends Game {
    @Override
    void initialize() {
        System.out.println("足球游戏初始化完成，请开始游戏");
    }

    @Override
    void startPlay() {
        System.out.println("足球游戏已开始");
    }

    @Override
    void endPlay() {
        System.out.println("足球游戏已结束");
    }
}
```

3. 使用 *Game* 的模板方法 play() 来演示游戏的定义方式。

> TemplatePatternDemo.java

```java
public class TemplatePatternDemo {
    public static void main(String[] args) {
        Game game = new Cricket();
        game.play();
        game = new Football();
        game.play();
    }
}
```

4. 输出结果

> 板球游戏初始化完成，请开始游戏
>
> 板球游戏已开始
>
> 板球游戏已结束
>
> 足球游戏初始化完成，请开始游戏
>
> 足球游戏已开始
>
> 足球游戏已结束

### 访问者模式

> 意图：主要将数据结构与数据操作分离。
>
> 主要解决：稳定的数据结构和易变的操作耦合问题。
>
> 应用实例：您在朋友家做客，您是访问者，朋友接受您的访问，您通过朋友的描述，然后对朋友的描述做出一个判断，这就是访问者模式。
>
> 使用场景： 1、对象结构中对象对应的类很少改变，但经常需要在此对象结构上定义新的操作。 2、需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而需要避免让这些操作"污染"这些对象的类，也不希望在增加新操作时修改这些类。

1. 定义一个表示元素的接口。

> ComputerPart.java

```java
public interface ComputerPart {
    void accept(ComputerPartVisitor computerPartVisitor);
}
```

2. 创建扩展了上述类的实体类。

> Mouse.java

```java
public class Mouse implements ComputerPart {
    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        computerPartVisitor.visit(this);
    }
}
```

> Keyboard.java

```java
public class Keyboard implements ComputerPart {
    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        computerPartVisitor.visit(this);
    }
}
```

> Monitor.java

```java
public class Monitor implements ComputerPart {
    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        computerPartVisitor.visit(this);
    }
}
```

> Computer.java

```java
public class Computer implements ComputerPart {

    ComputerPart[] parts;

    public Computer(){
        parts = new ComputerPart[]{new Mouse(),new Keyboard(),new Monitor()};
    }

    @Override
    public void accept(ComputerPartVisitor computerPartVisitor) {
        for (int i = 0; i < parts.length; i++) {
            parts[i].accept(computerPartVisitor);
        }
        computerPartVisitor.visit(this);
    }
}
```

3. 定义一个表示访问者的接口。

> ComputerPartVisitor.java

```java
public interface ComputerPartVisitor {
    void visit(Computer computer);
    void visit(Mouse mouse);
    void visit(Keyboard keyboard);
    void visit(Monitor monitor);
}
```

4. 定义一个表示访问者的接口。创建实现了上述类的实体访问者。

> ComputerPartDisplayVisitor.java

```java
public class ComputerPartDisplayVisitor implements ComputerPartVisitor{
    @Override
    public void visit(Computer computer) {
        System.out.println("显示计算机");
    }

    @Override
    public void visit(Mouse mouse) {
        System.out.println("显示鼠标");
    }

    @Override
    public void visit(Keyboard keyboard) {
        System.out.println("显示键盘");
    }

    @Override
    public void visit(Monitor monitor) {
        System.out.println("显示监视器");
    }
}
```

5. 使用 *ComputerPartDisplayVisitor* 来显示 *Computer* 的组成部分。

> VisitorPatternDemo.java

```java
public class VisitorPatternDemo {
    public static void main(String[] args) {
        ComputerPart computer = new Computer();
        computer.accept(new ComputerPartDisplayVisitor());
    }
}
```

6. 显示结果

>显示鼠标
>
>显示键盘
>
>显示监视器
>
>显示计算机

### 空对象模式

> 在空对象模式（Null Object Pattern）中，一个空对象取代 NULL 对象实例的检查。Null 对象不是检查空值，而是反应一个不做任何动作的关系。这样的 Null 对象也可以在数据不可用的时候提供默认的行为。
>
> 在空对象模式中，我们创建一个指定各种要执行的操作的抽象类和扩展该类的实体类，还创建一个未对该类做任何实现的空对象类，该空对象类将无缝地使用在需要检查空值的地方。

1. 创建一个抽象类。

```java
public abstract class AbstractCustomer {
    protected String name;
    public abstract boolean isNil();
    public abstract String getName();
}
```

2. 创建扩展了上述类的实体类。

> RealCustomer.java

```java
public class RealCustomer extends AbstractCustomer {

    public RealCustomer(String name) {
        this.name = name;
    }

    @Override
    public boolean isNil() {
        return false;
    }

    @Override
    public String getName() {
        return name;
    }
}
```

> NullCustomer.java

```java
public class NullCustomer extends AbstractCustomer {
    @Override
    public boolean isNil() {
        return false;
    }

    @Override
    public String getName() {
        return "不可用的值";
    }
}
```

3. 创建 *CustomerFactory* 类。

> CustomerFactory.java

```java
public class CustomerFactory {
    public static final String[] names = {"张三","李四","王五"};

    public static AbstractCustomer getCustomer(String name){
        for (int i = 0;i < names.length;i++){
            if (names[i].equalsIgnoreCase(name)){
                return new RealCustomer(name);
            }
        }
        return new NullCustomer();
    }
}
```

4. 使用 *CustomerFactory*，基于客户传递的名字，来获取 *RealCustomer* 或 *NullCustomer* 对象。

> NullPatternDemo.java

```java
public class NullPatternDemo {
    public static void main(String[] args) {
        AbstractCustomer customer1 = CustomerFactory.getCustomer("张三");
        AbstractCustomer customer2 = CustomerFactory.getCustomer("李四");
        AbstractCustomer customer3 = CustomerFactory.getCustomer("王五");
        AbstractCustomer customer4 = CustomerFactory.getCustomer("赵六");

        System.out.println("消费者");
        System.out.println(customer1.getName());
        System.out.println(customer2.getName());
        System.out.println(customer3.getName());
        System.out.println(customer4.getName());
    }
}
```

5. 输出结果

> 消费者
> 张三
> 李四
> 王五
> 不可用的值