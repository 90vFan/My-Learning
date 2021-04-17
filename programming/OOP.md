面向对象编程
-----------------

面向对象编程的英文缩写是 OOP，全称是 Object Oriented Programming

## 面向对象与面向过程

面向对象编程以**类**为组织代码的基本单元，面向过程编程则是以**过程（或方法）**作为组织代码的基本单元。

对于大规模复杂程序的开发，程序的处理流程并非单一的一条主线，而是错综复杂的网状结构。面向对象编程比起面向过程编程，更能应对这种复杂类型的程序开发。面向对象编程相比面向过程编程，具有更加丰富的特性（封装、抽象、继承、多态）。利用这些特性编写出来的代码，更加易扩展、易复用、易维护。从编程语言跟机器打交道的方式的演进规律中，我们可以总结出：面向对象编程语言比起面向过程编程语言，更加人性化、更加高级、更加智能。

### 三种违反面向对象编程风格的典型代码设计

1. 滥用 getter、setter 方法：违反访问权限
2. Constants 类、Utils 类的设计问题

尽量能做到**职责单一**，定义一些**细化的小类**，比如 RedisConstants、FileUtils，而不是定义一个大而全的 Constants 类、Utils 类。除此之外，如果能将这些类中的属性和方法，划分归并到其他业务类中，那是最好不过的了，能极大地提高类的内聚性和代码的可复用性。

**静态方法**一般用来操作**静态变量或者外部数据**。你可以联想一下我们常用的各种 **Utils 类**，里面的方法一般都会定义成静态方法，可以在不用创建对象的情况下，直接拿来使用。**静态方法将方法与数据分离，破坏了封装特性，是典型的面向过程风格**。

3. 基于贫血模型的开发模式

   面向过程设计，数据和操作是分开定义在 VO/BO/Entity 和 Controler/Service/Repository 中的。

## 四个特性

- 封装：也叫做**信息隐藏**或者**数据访问保护**。类通过暴露有限的访问接口，授权外部仅能通过类提供的方式（或者叫函数）来访问内部信息或者数据。访问权限控制，比如 Java 中的 private、protected、public 关键字
- 抽象：如何隐藏方法的具体实现，让调用者只需要关心方法提供了哪些功能，并不需要知道这些功能是如何实现的。
- 继承：继承是用来表示类之间的 is-a 关系。解决代码复用问题。
- 多态：多态是指，子类可以替换父类，在实际的代码运行过程中，调用子类的方法实现。实现方式：继承加方法重写，使用接口类语法， 使用 duck-typing 语法

### 多态

#### 接口类实现多态

``` java

public interface Iterator {
  boolean hasNext();
  String next();
  String remove();
}

public class Array implements Iterator {
  private String[] data;
  
  public boolean hasNext() { ... }
  public String next() { ... }
  public String remove() { ... }
  //...省略其他方法...
}

public class LinkedList implements Iterator {
  private LinkedListNode head;
  
  public boolean hasNext() { ... }
  public String next() { ... }
  public String remove() { ... }
  //...省略其他方法... 
}

public class Demo {
  private static void print(Iterator iterator) {
    while (iterator.hasNext()) {
      System.out.println(iterator.next());
    }
  }
  
  public static void main(String[] args) {
    Iterator arrayIterator = new Array();
    print(arrayIterator);
    
    Iterator linkedListIterator = new LinkedList();
    print(linkedListIterator);
  }
}
```

#### duck-typing 来实现多态

``` python
class Logger:
    def record(self):
        print(“I write a log into file.”)
        
class DB:
    def record(self):
        print(“I insert data into db. ”)
        
def test(recorder):
    recorder.record()

def demo():
    logger = Logger()
    db = DB()
    test(logger)
    test(db)
```

## 接口类 VS 抽象类

我们可以使用接口来实现面向对象的抽象特性、多态特性和基于接口而非实现的设计原则，使用抽象类来实现面向对象的继承特性和模板设计模式等等。

### 抽象类

**is-a **关系，j解决代码**复用**问题。

- 抽象类不允许被实例化，只能被继承
- 抽象类可以包含属性和方法。
- 子类继承抽象类，必须实现抽象类中的所有抽象方法。

### 接口类

**has-a** 关系，可称作协议（contract），或者称为 behaves like 关系。解决抽象问题，侧重于**解耦**。

接口仅仅是对方法的抽象，是一种 has-a 关系，表示具有某一组行为特性，是为了解决解耦问题，隔离接口和具体的实现，提高代码的扩展性。

- 接口不能包含属性（也就是成员变量）
- 接口只能声明方法，方法不能包含代码实现
- 类实现接口的时候，必须实现接口中声明的所有方法

### 基于接口而非实现编程原则

> 基于接口而非实现编程原则，Program to an interface, not an implementation

>  越抽象、越顶层、越脱离具体某一实现的设计，越能提高代码的灵活性，越能应对未来的需求变化。好的代码设计，不仅能应对当下的需求，而且在将来需求发生变化的时候，仍然能够在不破坏原有代码设计的情况下灵活应对。而抽象就是提高代码扩展性、灵活性、可维护性最有效的手段之一。

在做软件开发的时候，一定要有抽象意识、封装意识、接口意识。在定义接口的时候，不要暴露任何实现细节。接口的定义只表明做什么，而不是怎么做。而且，在设计接口的时候，我们要多思考一下，这样的接口设计是否足够通用，是否能够做到在替换具体的接口实现的时候，不需要任何接口定义的改动。

这条原则的设计初衷是，将接口和实现相分离，封装不稳定的实现，暴露稳定的接口。上游系统面向接口而非实现编程，不依赖不稳定的实现细节，这样当实现发生变化的时候，上游系统的代码基本上不需要做改动，以此来降低代码间的耦合性，提高代码的扩展性。

我们在定义接口的时候，一方面，命名要足够通用，**不能包含跟具体实现相关的字眼**；另一方面，与特定实现有关的方法不要定义在接口中。

### 组合优于继承原则

**通过组合、接口、委托三个技术手段，替换掉继承**

比如 is-a 关系，我们可以通过组合和接口的 has-a 关系来替代；多态特性我们可以利用接口来实现；代码复用我们可以通过组合和委托来实现。所以，从理论上讲，通过组合、接口、委托三个技术手段，我们完全可以替换掉继承，在项目中不用或者少用继承关系，特别是一些复杂的继承关系。

``` java

public interface Flyable {   // 接口
  void fly()；
}
public class FlyAbility implements Flyable {  // 接口实现
  @Override
  public void fly() { //... }
}
//省略Tweetable/TweetAbility/EggLayable/EggLayAbility

public class Ostrich implements Tweetable, EggLayable {      //接口
  private TweetAbility tweetAbility = new TweetAbility();    //组合
  private EggLayAbility eggLayAbility = new EggLayAbility(); //组合
  //... 省略其他属性和方法...
  @Override
  public void tweet() {
    tweetAbility.tweet(); // 委托
  }
  @Override
  public void layEgg() {
    eggLayAbility.layEgg(); // 委托
  }
}
```

#### Q&A

1. 为什么不推荐使用继承？

虽然继承有诸多作用，但继承层次过深、过复杂，也会影响到代码的可维护性

2. 组合相比继承有哪些优势？

继承主要有三个作用：表示 is-a 关系，支持多态特性，代码复用。而这三个作用都可以通过组合、接口、委托三个技术手段来达成。除此之外，利用组合还能解决层次过深、过复杂的继承关系影响代码可维护性的问题。

3. 如何判断该用组合还是继承？

果类之间的继承结构稳定，层次比较浅，关系不复杂，我们就可以大胆地使用继承。反之，我们就尽量使用组合来替代继承。除此之外，还有一些设计模式、特殊的应用场景，会固定使用继承或者组合。

> .接口+组合+委托符合矢量化思想，那就是将物体特征分成不同的维度，每个维度独立变化。继承则是将物体分类，抽取共性，处理共性，操作的灵活性大打折扣，毕竟现实中的物体特征多，共性少。

## 充血模型

充血模型（Rich Domain Model），数据和对应的业务逻辑被封装到同一个类中。

领域驱动设计，即 DDD，主要是用来指导如何解耦业务系统，划分业务模块，定义业务领域模型及其交互。

贫血模型（Anemic Domain Model），数据与操作分离，破坏了面向对象的封装特性，是一种典型的面向过程的编程风格。

MVC 三层架构中的 M 表示 Model，V 表示 View，C 表示 Controller。它将整个项目分为三层：数据层、展示层、逻辑层。后端项目通常分为 Repository 层、Service 层、Controller 层。其中，Repository 层负责数据访问，Service 层负责业务逻辑，Controller 层负责暴露接口

``` java

////////// Controller+VO(View Object) //////////
public class UserController {
  private UserService userService; //通过构造函数或者IOC框架注入
  
  public UserVo getUserById(Long userId) {
    UserBo userBo = userService.getUserById(userId);
    UserVo userVo = [...convert userBo to userVo...];
    return userVo;
  }
}

public class UserVo {//省略其他属性、get/set/construct方法
  private Long id;
  private String name;
  private String cellphone;
}

////////// Service+BO(Business Object) //////////
public class UserService {
  private UserRepository userRepository; //通过构造函数或者IOC框架注入
  
  public UserBo getUserById(Long userId) {
    UserEntity userEntity = userRepository.getUserById(userId);
    UserBo userBo = [...convert userEntity to userBo...];
    return userBo;
  }
}

public class UserBo {//省略其他属性、get/set/construct方法
  private Long id;
  private String name;
  private String cellphone;
}

////////// Repository+Entity //////////
public class UserRepository {
  public UserEntity getUserById(Long userId) { //... }
}

public class UserEntity {//省略其他属性、get/set/construct方法
  private Long id;
  private String name;
  private String cellphone;
}
```

