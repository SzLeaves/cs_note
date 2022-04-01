# 201007. 抽象类

## 抽象方法
由于继承树中多态的存在，每个子类都可以覆写父类的方法，例如：
```java
class Person {
    public void run() { … }
}

class Student extends Person {
    @Override
    public void run() { … }
}

class Teacher extends Person {
    @Override
    public void run() { … }
}
```

从Person类派生的Student和Teacher都可以覆写run()方法。  

如果父类Person的run()方法没有实际意义，能否去掉方法的执行语句？  
```java
class Person {
    public void run(); // Compile Error!
}
```

答案是不行，会导致编译错误，因为定义方法的时候，必须实现方法的语句  

能不能去掉父类的run()方法？  
答案还是不行，因为去掉父类的run()方法，就失去了多态的特性  
例如，`runTwice()`就无法编译：
```java
// 只有父类存在的方法才能进行重写
public void runTwice(Person p) {
    p.run(); // Person没有run()方法，会导致编译错误
}
```

如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名  
目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法：
```java
class Person {
    public abstract void run();
}
```

抽象方法本身没有实现任何方法语句  
因为这个抽象方法本身是无法执行的，所以，Person类也**无法被实例化**  
**必须把Person类本身也声明为abstract，才能正确编译它：**  
```java
abstract class Person {
    public abstract void run();
}
```

## 抽象类
如果一个class定义了方法，但没有具体执行代码  
这个方法就是抽象方法，抽象方法用abstract修饰  

因为无法执行抽象方法，因此这个类也必须申明为**抽象类（abstract class）**  
使用abstract修饰的类就是抽象类。我们无法实例化一个抽象类：
```java
Person p = new Person(); // 编译错误
```

因为抽象类本身被设计成只能用于被继承  
因此，抽象类可以强迫子类实现其定义的抽象方法（**即强制要求子类Override父类的抽象方法**），否则编译会报错  
> 抽象方法实际上相当于定义了“规范”  

例：Person类定义了抽象方法run()  
那么，在实现子类Student的时候，就必须覆写run()方法：
```java
// abstract class
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run();
    }
}

abstract class Person {
    public abstract void run();
}

class Student extends Person {
    // 继承抽象类的子类必须重写对应的父类方法
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

当然，抽象类内部可以包含非抽象方法和普通字段（当然包括构造方法）：
```java
abstract class Person {
    // 字段
    protected int var;
    
    // 非抽象方法
    public Person(...) { ... }
    public void fun1(...) { ... }
    ...
    // 抽象方法
    public abstract void run();
}
```

> 抽象类本身无法实例化，不过可以使用匿名类这一特性直接对抽象类进行"实例化"  
> 这个时候需要在匿名类中定义（或者说是Override）这个类已有的抽象方法，像这样：  
> ```java
> abstract class Person {
>     public abstract void run();
> }
> 
> public class Main {
>     public static void main(String[] args) {
>           // 创建匿名类，绕过抽象定义
>           Person p = new Person {
>                public void run() {...}  // 没问题
>           }
>     }
> }
> ```
> 当然，此时被真正实例化的也不是原本的抽象类Person  


## 面向抽象编程
当我们定义了抽象类Person，以及具体的Student、Teacher子类的时候  
我们可以通过抽象类Person类型去引用具体的子类的实例：
```java
Person s = new Student();
Person t = new Teacher();
```

这种引用抽象类的好处在于：  
**对其进行方法调用，并不用关心Person类型变量的具体子类型**：
```java
// 不关心Person变量的具体子类型:
s.run();
t.run();
```

如果引用的是一个新的子类，仍然不用关心具体类型：
```java
// 同样不关心新的子类是如何实现run()方法的：
Person e = new Employee();
e.run();
```

这种尽量引用高层类型，避免引用实际子类型的方式，称之为*面向抽象编程*  


面向抽象编程的本质就是：  
* 上层代码只定义规范（例如：abstract class Person）
* 不需要子类就可以实现业务逻辑（正常编译）
* 具体的业务逻辑由不同的子类实现，调用者并不关心


## 小结
* 通过abstract定义的方法是抽象方法，它只有定义，没有实现  
  抽象方法定义了子类必须实现的接口规范
* **定义了抽象方法的class必须被定义为抽象类，从抽象类继承的子类必须实现抽象方法**
* 如果不实现抽象方法，则该子类仍是一个抽象类
* 面向抽象编程使得调用者只关心抽象方法的定义，不关心子类的具体实现
