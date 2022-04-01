# 200919.构造方法

创建实例的时候，我们经常需要同时初始化这个实例的字段，例如：
```java
Person ming = new Person();
ming.setName("小明");
ming.setAge(12);
```

初始化对象实例需要3行代码，而且，如果忘了调用setName()或者setAge()，这个实例内部的状态就是不正确的。

如果要在**创建对象实例的时候就把该对象内部的成员字段初始化**  
就需要**构造方法**

创建实例的时候，实际上是通过构造方法来初始化实例的  
我们先来定义一个构造方法，能在创建Person实例的时候，一次性传入name和age  
完成初始化：
```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person("Xiao Ming", 15);
        System.out.println(p.getName());
        System.out.println(p.getAge());
    }
}

class Person {
    private String name;
    private int age;
    
    //构造方法--无返回值，名称即类名
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}
```

由于构造方法是如此特殊，所以**构造方法的名称就是类名**  
构造方法的参数没有限制，在方法内部，也可以编写任意语句  
但是***构造方法没有返回值（也没有void），调用构造方法，必须用new操作符***

## 默认构造方法
如果一个类没有定义构造方法，**编译器会自动为我们生成一个默认构造方法**  
**它没有参数，也没有执行语句，类似这样：**
```java
class Person {
     public Person() { }
}
```

但如果我们**自定义**了一个构造方法，**编译器将不再自动创建默认构造方法**  
如果既要能使用带参数的构造方法，又想保留不带参数的构造方法  
**那么只能把两个构造方法都定义出来：**
```java
public class Main {
    public static void main(String[] args) {
         Person p1 = new Person("Xiao Ming", 15); // 既可以调用带参数的构造方法
         Person p2 = new Person(); // 也可以调用无参数构造方法
    }
}
```
按照多态的特性，创建对象实例时  
编译器会根据传入构造方法的参数调用对应的构造方法


没有在构造方法中初始化字段时，引用类型的字段默认是null  
数值类型的字段用默认值，int类型默认值是0，布尔类型默认值是false：
```java
class Person {
     private String name; // 默认初始化为null
     private int age; // 默认初始化为0

     public Person() {}
}
```

也可以对字段直接进行初始化：
```
class Person {
     private String name = "Unamed";
     private int age = 10;
}
```

在Java中，创建对象实例的时候，按照如下顺序进行初始化：
  1. 先初始化字段，例如:  
 `int age = 10`表示字段初始化为10  
  `double salary`表示字段默认初始化为0  
  `String name`表示引用类型字段默认初始化为null

  2. 执行构造方法的代码进行初始化

因此，构造方法的代码最后运行  
所以`new Person("Xiao Ming", 12)`的字段值**最终由构造方法的代码(参数)确定**
```java
class Person {
     private String name = "Unamed";
     private int age = 10;


     public Person(String name, int age) {
         // 以下代码决定name和age字段的最终值
         this.name = name;
         this.age = age;     
     }
}
```

## 多构造方法
可以定义多个构造方法，在通过new操作符调用的时候  
编译器通过构造方法的参数数量、位置和类型自动区分：
```java
class Person {
     private String name;
     private int age;
    
     // 构造方法的重载(@Overload)属性
     public Person(String name, int age) {
         this.name = name;
         this.age = age;
     }

     public Person(String name) {
         this.name = name;
         this.age = 12;
     }

     public Person() { }
}
```

如果调用`new Person("Xiao Ming", 20)`，会自动匹配到构造方法`public Person(String, int)`  
如果调用`new Person("Xiao Ming")`，会自动匹配到构造方法`public Person(String)`  
如果调用`new Person()`，会自动匹配到构造方法`public Person()`

一个构造方法可以调用其他构造方法  
**这样做的目的是便于代码复用**，调用其他构造方法的语法是`this(…)`：
```java
class Person {
     private String name;
     private int age;

     public Person(String name, int age) {
          this.name = name;
          this.age = age;
     }

     public Person(String name) {
         this(name, 18); // 调用另一个构造方法Person(String, int)
     }

     public Person() {
         this("Unnamed"); // 调用另一个构造方法Person(String)
     }
}
```

## 小结
* 实例在创建时通过new操作符会调用其对应的构造方法，构造方法用于初始化实例  
* 没有定义构造方法时，编译器会自动创建一个默认的无参数构造方法  
* 可以定义多个构造方法，编译器根据参数自动判断  
* 可以在一个构造方法内部调用另一个构造方法，便于代码复用
