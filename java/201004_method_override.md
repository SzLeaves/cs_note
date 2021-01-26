# 201004.方法重写(@Override)、多态
在<u>**继承关系**</u>中  
**子类**如果定义了一个与**父类方法签名完全相同**的方法，称为**覆写/重写（Override）**

例如，在Person类中，我们定义了run()方法：
```java
class Person {
     public void run() {
         System.out.println("Person.run");
     }
}
```

在子类Student中，覆写这个run()方法：
```java
class Student extends Person {
@Override
public void run() {
     System.out.println("Student.run");
     }    
}
```

`Override`和`Overload`不同的是：
* 如果方法签名不同，就是`Overload`，`Overload`方法是一个新方法  
* 如果**方法签名相同，并且返回值也相同**，就是`Override`

> 方法名相同，方法参数相同，但方法返回值不同，也是不同的方法  
在Java程序中，出现这种情况，编译器会报错。

```java
class Person {
     public void run() { … }
}

class Student extends Person {
     // 不是Override，因为参数不同:
     public void run(String s) { … }

     // 不是Override，因为返回值不同:
     public int run() { … }
}
```

**加上`@Override`可以让编译器帮助检查是否进行了正确的覆写**  
希望进行覆写，但是不小心写错了方法签名，编译器会报错
```java
// override check
public class Main {
    public static void main(String[] args) {
    }
}

class Person {
    public void run() {}
}

public class Student extends Person {
    @Override // Compile error!
    public void run(String s) {}     //父类同方法中没有参数
}
```

> `@Override`不是必需的



## 多态
在多态的环境下，引用变量的声明类型可能与其实际类型不符，例如：  
```java
Person p = new Student();
```

现在考虑一种情况，如果子类覆写了父类的方法：
```java
// override
public class Main {
     public static void main(String[] args) {
        Person p = new Student();   //向上转型
        p.run(); // 应该打印Person.run还是Student.run?
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

在这里，变量p的引用类型为Person，实际（指向）类型为Student  
实际上p变量调用的成员方法是Student的run()方法。因此可得出结论：  

> Java的实例方法调用是基于**运行时的实际类型的动态调用**，而非变量的声明类型  

这个非常重要的特性在面向对象编程中称之为**多态(Polymorphic)**  


多态是指，针对某个类型的方法调用  
其真正执行的方法取决于**运行时期实际类型的方法**，例如：
```java
Person p = new Student();
p.run();
```
显然，上面的代码示例中，p最后调用的是Student里的run方法


但假设我们编写这样一个方法：
```
public void runTwice(Person p) {
     // 调用的是哪个类中的方法？
     p.run();
     p.run();
}
```
它传入的**参数类型是Person**  
我们是无法知道传入的参数实际类型究竟是指向Person  
还是Student，还是Person的其他子类  
因此，无法确定调用的是不是Person类定义的run()方法

**所以，多态的特性就是**：  
* 运行期才能动态决定调用的子类方法  
* 对某个类型调用某个方法，**执行的实际方法可能是某个子类的覆写方法**  

## 示例
假设我们定义一种收入，需要给它报税，那么先定义一个Income类：
```java
class Income {
protected double income;
     public double getTax() {
         return income * 0.1; // 税率10%
     }
}
```

对于工资收入，可以减去一个基数  
那么我们可以从Income派生出SalaryIncome，并覆写getTax()：
```java
class Salary extends Income {
@Override
     public double getTax() {
         if (income <= 5000) {
         return 0;
         }
         return (income - 5000) * 0.2;
     }
}
```

如果你享受国务院特殊津贴，那么按照规定，可以全部免税：
```java
class StateCouncilSpecialAllowance extends Income {
@Override
     public double getTax() {
         return 0;
     }
}
```

现在，我们要编写一个报税的财务软件  
对于一个人的所有收入进行报税，可以这么写：
```java
// Polymorphic
public class Main {
    public static void main(String[] args) {
    /* 给一个有普通收入、工资收入和享受国务院特殊津贴的小伙伴算税 */

    // 新建一个Income类型的数组并初始化（包含）三个类型的实例
    // 使用new操作符新建实例，并使用构造方法
    Income[] incomes = new Income[] {
        new Income(3000),     
        new Salary(7500),
        new StateCouncilSpecialAllowance(15000)
    };
     /*
          以上等同于：
          Income incomes_01 = new Income(3000);
          Income incomes_02 = new Salary(7500);
          Income incomes_03 = new StateCouncilSpecialAllowance(15000);
          使用数组声明的同时也使用了向上转型
     */

    System.out.println(totalTax(incomes));

    public static double totalTax(Income... incomes) {
        double total = 0;

        //使用for each循环进行遍历
        for (Income income: incomes) {
            total = total + income.getTax();
        }

        return total;
    }
}

/* 注意Income类与其他两个子类的继承关系树 */

// 普通收入
class Income {
    protected double income;

    public Income(double income) {
        this.income = income;
    }

    public double getTax() {
        return income * 0.1; // 税率10%
    }
}

// 工资收入
class Salary extends Income {
    public Salary(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}

// 津贴
class StateCouncilSpecialAllowance extends Income {
    public StateCouncilSpecialAllowance(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        return 0;
    }
}
```

观察totalTax()方法：  
利用多态，totalTax()方法只需要和Income打交道  
它完全不需要知道Salary和StateCouncilSpecialAllowance的存在  
就可以正确计算出总税  

如果我们要新增一种稿费收入，只需要从Income派生  
然后正确覆写getTax()方法就可以。把新的类型传入totalTax()  
不需要修改任何代码

> 多态具有一个非常强大的功能，就是允许添加更多类型的子类实现功能扩展  
却不需要修改基于父类的代码


## 覆写Object方法
因为所有的class最终都继承自Object，而Object定义了几个重要的方法：
* `toString()` 把instance输出为String
* `equals()` 判断两个instance是否逻辑相等
* `hashCode()` 计算一个instance的哈希值

在必要的情况下，我们可以覆写Object的这几个方法。例如：
```java
// 显示更有意义的字符串
@Override
public String toString() {
     return "Person:name=" + name;
}
```

```java
// 比较是否相等:
@Override
public boolean equals(Object o) {
     // 当且仅当o为Person类型:
     if (o instanceof Person) {
         Person p = (Person) o;
         // 并且name字段相同时，返回true:
         return this.name.equals(p.name);
     }
     return false;
}
```

```java
// 计算hash:
@Override
public int hashCode() {
     return this.name.hashCode();
}
```

## 调用`super`
在子类的覆写方法中，如果要调用父类的被覆写的方法  
可以通过super来调用。例如：
```java
class Person {
     protected String name;
     public String hello() {
         return "Hello, " + name;
     }
}

class Student extends Person {
     @Override
     public String hello() {
         // 调用父类的hello()方法:
         return super.hello() + "!";
     }
}
```

## 修饰符`final`
继承可以允许子类覆写父类的方法  
如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为final  
**用final修饰的方法不能被Override：**  
```java
class Person {
     protected String name;
     public final String hello() {
         return "Hello, " + name;
     }
}

Student extends Person {
     // compile error: 不允许覆写
     @Override
     public String hello() { }
}
```

如果一个类**不希望任何其他类继承自它**，那么可以把这个类本身标记为final  
**用final修饰的类不能被继承：**  
```java
final class Person {
     protected String name;
}

// compile error: 不允许继承自Person
Student extends Person { }
```

对于一个类的实例字段，同样可以用final修饰  
**用final修饰的字段在初始化后不能被修改：**  
```java
class Person {
     public final String name = "Unamed";
}
```
对final字段重新赋值会报错,相当于为该字段添加了**只读属性**，成为常量：  
```java
Person p = new Person();
p.name = "New Name"; // compile error!
```

*可以在构造方法中初始化final字段：*
```java
class Person {
     public final String name;
     public Person(String name) {
         this.name = name;
     }
}
```
这种方法更为常用，因为可以保证实例一旦创建，其final字段就不可修改


## 小结
* 子类可以覆写父类的方法（Override），覆写在子类中改变了父类方法的行为
* Java的方法调用总是作用于运行期对象的实际类型，这种行为称为多态
* final修饰符有多种作用：
    + final修饰的方法可以阻止被覆写
    + final修饰的class可以阻止被继承
    + final修饰的field必须在创建对象时初始化，随后不可修改
