# 200923.继承(派生)、转型

定义Person类：
```java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}

    public int getAge() {...}
    public void setAge(int age) {...}
}
```

现在，假设需要定义一个Student类，字段如下：
```java
class Student {
    private String name;
    private int age;
    private int score;

    public String getName() {...}
    public void setName(String name) {...}

    public int getAge() {...}
    public void setAge(int age) {...}

    public int getScore() { … }
    public void setScore(int score) { … }
}
```

仔细观察，发现Student类包含了Person类已有的字段和方法  
只是多出了一个score字段和相应的`getScore()、setScore()`方法

实际上，在Student中不用写重复的代码，这时候就需要**继承(inheritance)**  

继承是面向对象编程中非常强大的一种机制，它首先可以复用代码  
当我们让Student从Person继承时，**Student就获得了Person的所有功能**  
我们只需要为Student编写新增的功能  
> **继承的所有功能**包括父类的成员字段和成员变量

**Java使用`extends`关键字来实现继承：**
```java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}

class Student extends Person {
    // 不要重复name和age字段/方法,
    // 只需要定义新增score字段/方法:
    private int score;

    public int getScore() { … }
    public void setScore(int score) { … }
}
```

可见，通过继承，Student只需要编写额外的功能，不再需要写重复代码

> **注意**：继承的子类自动获得了父类的所有字段和方法  
**不能定义与父类重名的字段（方法可以重载）**

在OOP的术语中:  
* `Person`称为**超类（super class），父类（parent class），基类（base class）**  
* `Student`称为**子类（subclass），扩展类（extended class）**  

## 继承树
注意到我们在定义**父类Person**的时候，没有写extends  
在Java中，<u>没有明确写extends的类，**编译器会自动加上extends Object**</u>  

所以，任何类，除了Object，都会继承自某个类  
**Object是所有类的父类**  

下图是Person、Student的继承树：
```
┌───────────┐
│  Object   │
└───────────┘
      ▲
      │
┌───────────┐
│  Person   │
└───────────┘
      ▲
      │
┌───────────┐
│  Student  │
└───────────┘
```
**Java只允许一个class继承自一个类(单继承)**  
**因此，一个类有且仅有一个父类**，只有Object特殊，它没有父类  
类似的，如果我们定义一个继承自Person的Teacher，它们的继承树关系如下：
```
       ┌───────────┐
       │  Object   │
       └───────────┘
             ▲
             │
       ┌───────────┐
       │  Person   │
       └───────────┘
          ▲     ▲
          │     │
          │     │
┌───────────┐ ┌───────────┐
│  Student  │ │  Teacher  │
└───────────┘ └───────────┘
```

![alt 00](images/200923.inheritance/00.png)

## 关键字：protected
继承有个特点，**就是子类无法访问父类的private字段或者private方法**  
例如，Student类就无法访问Person类的name和age字段：
```java
class Person {
    private String name;
    private int age;
}

class Student extends Person {
    public String hello() {
       return "Hello, " + name; // 编译错误：无法访问name字段
    }
}
```

这使得继承的作用被削弱了  
为了让子类可以访问父类的字段，需要把private改为protected  

用protected修饰的字段可以被子类访问：
```java
class Person {
    protected String name;
    protected int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // OK!
    }
}
```

protected关键字可以把<u>字段和方法的访问权限</u>**控制在继承树内部**   
所以一个protected字段和方法可以被其子类，以及子类的子类所访问


## 关键字：super
super关键字表示父类（超类）  
子类引用父类的字段/方法时，可以用`super.fieldName`

例如：
```java
class Student extends Person {
    public String hello() {
        return "Hello, " + super.name;
    }
}
```

如果需要调用父类的普通方法，可以用`super.method()`  
> 如果是构造方法，则是`super()`  

实际上，这里使用`super.name`，或者`this.name`，或者`name`  
效果都是一样的  
编译器会自动定位到父类的name字段

但是，在某些时候，就必须使用super：
```java
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Xiao Ming", 12, 89);
    }
}

class Person {
    protected String name;
    protected int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class Student extends Person {
    protected int score;

    // 注意继承类的构造方法与父类构造方法的区别
    public Student(String name, int age, int score) {
        this.score = score;   // error
    }
}
```

运行上面的代码，会得到一个编译错误  
大意是在Student的构造方法中，无法调用Person的构造方法  

这是因为在Java中，**任何class的构造方法，第一行语句必须是调用父类的构造方法**  
如果没有明确地调用父类的构造方法，编译器会自动加一句`super()`  

所以Student类的构造方法实际上是这样：  
```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(); // 自动调用父类的构造方法
        this.score = score;
    }
}
```

但是，Person类并没有无参数的构造方法，因此，编译失败  

解决方法是调用Person类存在的某个构造方法：
```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age); // 调用父类的构造方法Person(String, int)
        this.score = score;
    }
}
```
这样就可以正常编译了

或者，如果想给这个继承类保留没有参数的构造方法  
就需要在<u>**父类**</u>中把两个构造方法都写出来：
```java
class Person {
    protected String name;
    protected int age;

    // 没有参数的构造方法
    public Person() {

    }

    // 有参数的构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```
这样在子类中就能正常调用了：
```java
    public Student(String name, int age, int score) {
        super(); // 可以正常调用了
        this.score = score;
    }
```

> * 如果父类没有默认的构造方法，子类就必须显式调用super()并给出参数以便让编译器定位到父类的一个合适的构造方法  

> * 只要有class继承自某一个父类，就需要注意父类是否存在有定义的构造方法，如果有，则用super()在子类构造方法中显式调用  

> * **子类不会继承任何父类的构造方法**  
>   子类默认的构造方法是编译器自动生成的，不是继承的  

> * 调用格式(在子类构造方法中)：`super( [父类的构造方法中的参数] )`


## 阻止继承 (Java 15)
正常情况下，**只要某个class没有final修饰符**  
那么任何类都可以从该class继承

从Java 15开始，允许使用`sealed`修饰class  
并通过`permits`明确写出能够从该class继承的子类名称  

例如，定义一个Shape类：
```java
//创建一个sealed类型，名为Shape的class，并限定三个类继承它
public sealed class Shape permits Rect, Circle, Triangle {
    ...
}
```

上述Shape类就是一个`sealed`类，**它只允许指定的3个类继承它**。如果写：
```java
//final标识符，继承class Shape的新类class Rect
public final class Rect extends Shape {...}
```
是没问题的，因为Rect出现在Shape的`permits`列表中  
但是，如果定义一个Ellipse就会报错：
```java
public final class Ellipse extends Shape {...}
// Compile error: class is not allowed to extend sealed class: Shape
```
原因是Ellipse并未出现在Shape的`permits`列表中  
**这种`sealed`类主要用于一些框架，防止继承被滥用**

> `sealed`类在Java 15中目前是预览状态，要启用它，  
必须使用VM参数`--enable-preview`和`--source 15`


## 向上转型
如果一个引用变量的类型是Student，那么它可以指向一个Student类型的实例：
```java
Student s = new Student();
```
如果一个引用类型的变量是Person，那么它可以指向一个Person类型的实例：
```java
Person p = new Person();
```
现在问题来了：如果Student是从Person继承下来的  
那么，一个引用类型为Person的变量，能否指向Student类型的实例？
```java
Person p = new Student(); // ???
```
因为Student继承自Person(父类)，因此，它拥有Person的全部功能  
Person类型的变量，如果指向Student类型的实例，对它进行操作，是没有问题的

这种把一个<u>子类类型安全地变为父类类型的赋值</u>，被称为**向上转型（upcasting）**  

向上转型实际上是把一个子类型安全地变为更加抽象的父类型：
```java
Student s = new Student();
Person p = s; // upcasting, ok
Object o1 = p; // upcasting, ok
Object o2 = s; // upcasting, ok
```

注意到继承树是`Student -> Person -> Object`  
所以，可以把Student类型转型为Person，或者更高层次的Object

**向上转型会丢失子类的新增方法，同时会保留子类重新定义的方法**  
> *子类与父类中若有同名方法，则子类方法会重写父类方法*

向上转型对象也是父类创建的对象，而是子类对象的“简化”状态  
它不关心子类新增的功能，只关心子类继承和重写的功能

例：
```java
//父类
public class Person {
    public void eat(){
        System.out.println("Person eatting...");
    }     
    public void sleep() {
        System.out.println("Person sleep...");
    }
}

//继承自Person的子类
public class Superman extends Person{
    public void eat() {
        System.out.println("Superman eatting...");
    }

    public void fly() {
        System.out.println("Superman fly...");
    }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Superman(); // 向上转型
        person.sleep(); // 调用的是父类person的方法
        person.eat();   // 调用的是Superman里面的eat方法，它重写了父类方法
        person.fly();   // 报错，丢失了Superman类的fly方法
    }
}
```

![alt 01](images/200923.inheritance/01.png)

## 向下转型
和向上转型相反，如果把一个<u>父类类型强制转型为子类类型</u>  
就是**向下转型（downcasting）**  

> **向下转型必须是在<u>已经向上转型的子类</u>的基础上进行**  

向下转型可以得到子类的所有方法（包含父类的方法），例如：
```java
Person p1 = new Student(); // upcasting, ok
Person p2 = new Person();  //声明了一个父类实例
Student s1 = (Student) p1; // ok  
Student s2 = (Student) p2; // runtime error! 父类实例不能向下转型
```
如果测试上面的代码，可以发现：

* Person类型p1实际指向Student实例  
Person类型变量p2实际指向Person实例  

* 在向下转型的时候，把p1转型为Student会成功  
因为p1确实指向Student实例  

* 把p2转型为Student会失败，因为p2的实际类型是Person  
因为p2实际指向的类型属于父类，**不能把父类直接转型变为子类**  
子类功能比父类多，多的功能无法凭空变出来

**因此，向下转型很可能会失败**  
失败的时候，Java虚拟机会报`ClassCastException`

为了避免向下转型出错，Java提供了`instanceof`操作符  
可以先判断一个实例究竟是不是某种类型：
```java
// 新建一个Person对象的实例p，并指向Person对象
Person p = new Person();
System.out.println(p instanceof Person); // true
System.out.println(p instanceof Student); // false

// 新建一个Student对象的实例s，并指向Student对象
Student s = new Student();
System.out.println(s instanceof Person); // true
System.out.println(s instanceof Student); // true

// 新建一个Student对象的实例n，并指向null
Student n = null;
System.out.println(n instanceof Student); // false
```

**`instanceof`判断一个变量所指向的实例是否是指定类型，或者这个类型的子类**  
> 如果一个引用变量为null，那么对任何`instanceof`的判断都为false

利用`instanceof`，在向下转型前可以先判断：
```java
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```
<u>从Java 14开始，判断`instanceof`后，可以直接转型为指定变量</u>  
避免再次强制转型。例如，对于以下代码：
```java
Object obj = "hello";
if (obj instanceof String) {
    String s = (String) obj;
    System.out.println(s.toUpperCase());
}
```

可以改写如下：
```java
// instanceof variable:
Object obj = "hello";
if (obj instanceof String s) {
    // 可以直接使用变量s:
    System.out.println(s.toUpperCase());
}
```
这种使用`instanceof`的写法更加简洁。

向下转型示例：
```java
public class Main {
    public static void main(String[] args) {
        Person person = new Superman();
        Superman s = (Superman)person; //向下转型
        s.sleep();
        s.fly();
        s.eat();
    }
}
```
```
运行结果：
Person sleep…
Superman fly…
Superman eatting…
```
> 实际上，向下转型等价于：  
> `Person p = new Student()` ==  `Student s = new Student()` + `Peoson p = s`

![alt 02](images/200923.inheritance/02.png)

## 区分继承和组合
在使用继承时，我们要注意逻辑一致性  
考察下面的Book类：
```java
class Book {
    protected String name;
    public String getName() {...}
    public void setName(String name) {...}
}
```

这个Book类也有name字段，那么，我们能不能让Student继承自Book呢？
```java
class Student extends Book {    // ???
    protected int score;
}
```

显然，从逻辑上讲，这是不合理的  
**Student不应该从Book继承，而应该从Person继承**  

究其原因，是因为Student是Person的一种  
它们是is关系，而Student并不是Book  
**实际上Student和Book的关系是has关系**  

具有has关系不应该使用继承，而是使用**组合**，即Student可以持有一个Book实例：
```java
class Student extends Person {
    protected Book book;
    protected int score;
}
```
因此，继承是is关系，组合是has关系


## FAQ
### 1. 为什么使用继承
从已有的类派生出新的类，称为继承  
在不同的类中也可能会有共同的特征和动作  
可以把这些共同的特征和动作放在一个类中，让其它类共享  
因此可以定义一个通用类，然后将其扩展为其它多个特定类  
这些特定类继承通用类中的特征和动作

继承是 Java 中实现代码重用的重要手段，避免重复，易于维护，易于理解

### 2. 父类和子类
如果类 B 从类 A 派生，或者说类 B 扩展自类 A，或者说类 B 继承类 A  
则称类 A 为"父类"，也称为超类、基类  
称类 B 为"子类"，也称为次类、扩展类、派生类  
子类从它的父类中继承可访问的数据域和方法  
也可以添加新的数据域和新的方法

定义继承的语法：`修饰符 class 子类名 extends 父类名`

例如：Shape 类是父类，其子类可以有 Circle 类、Rectangle 类、Triangle 类，等等

### 3.继承的注意点
* **子类不是父类的子集**，子类一般比父类包含更多的数据域和方法  

* **父类中的 private 数据域在子类中是不可见的**  
在子类中不能直接使用它们，如果需要使用则应改用`protected`修饰  

* **继承是为"is-a"的关系建模的**  
父类和其子类间必须存在`"是一个"`的关系，否则不应用继承  

* **不是所有"is-a"的关系都应该用继承**  
例如，正方形是一个矩形，但不能让 Square 类来继承 Rectangle 类  
因为正方形不能从矩形扩展得到任何东西  
正确的继承关系是 Square 类继承 Shape 类  

* **区分继承[is-a]和组合[has-a]关系，防止继承滥用**  

* **Java 只允许单一继承（即一个子类只能有一个直接父类）**  
C++ 可以多继承（即一个子类有多个直接父类）

### 4.super 关键字
super表示使用它的类的父类。super 可用于：  
* 调用父类的构造方法；
* 调用父类的方法（子类覆盖了父类的方法时）；
* 访问父类的数据域（可以这样用但没有必要这样用）。
* 调用父类的构造方法语法：

用法：`super(); 或   super(父类的参数列表);`

注意：
* **super 语句必须是<u>子类构造方法的第一条语句</u>**  
不能在子类中使用父类构造方法名来调用父类构造方法  

* **父类的构造方法不被子类继承**  
调用父类的构造方法的唯一途径是使用 super 关键字  
如果子类中没显式调用，则编译器自动将`super();`作为子类构造方法的第一条语句  
这会形成一个构造方法链

* **静态方法中不能使用 super 关键字**  
因为静态类不属于任何实例，也无法访问实例中的字段与方法

调用父类的方法语法：`super.方法名(参数列表);`

> 如果是继承的方法，可以直接调用   
但如果**子类覆盖或重写了父类的方法**  
**则只有使用`super`才能在子类中调用父类中的被重写的方法**

### 5.this 关键字
this 关键字表示当前对象。可用于：
* **调用当前类的构造方法，并且必须是方法的第一条语句**  
如：`this();` 调用默认构造方法  
    `this(参数);` 调用带参构造方法

* **限定当前对象成员的变量/方法**  
一般用于**方法内的局部变量/方法**与**对象的成员变量/方法**同名的情况  
如：`this.num = num`  
其中`this.num` 表示当前对象的数据域变量`num`，而`num`表示方法中的局部变量


## 小结
* 继承是面向对象编程的一种强大的代码复用方式  
* Java只允许单继承，所有类最终的根类是Object  
* protected允许子类访问父类的字段和方法  
* 子类的构造方法可以通过super()调用父类的构造方法  
* 可以安全地向上转型为更抽象的类型  
* 可以强制向下转型，最好借助instanceof判断  
* 子类和父类的关系是is，has关系不能用继承  
