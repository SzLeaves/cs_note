# 201004.重写（@Override）与重载（@Overload）
方法的重写和重载都是面向对象程序中多态的一种实现策略  
<u>**他们都发生在方法上**</u>  

## 方法重写/Override(继承关系中实现)
在<u>子类继承父类后</u>：  
* 子类中存在与父类同名的方法  
* 方法中**参数的个数，顺序，类型与必须一致**  
这被称为方法重写
```java
public class Animal {
    public void eat() {
        System.out.pritnln("吃饭");
    }     
}

public class Dog extends Animal {
    @Override
    public void eat(){
        System.out.println("啃骨头");
    }
}
```

这里子类重写父类方法后，再次调用方法eat时均调用子类中重写的新方法`eat()`

## 方法重载/Overload(同一个class/继承关系中实现)
方法的重载是面向对象程序多态的一种实现策略  
表现是在同一个类中的多个同名方法的不同体现形式  

与重写不同的是：
* 方法重载发生在**同一个类**或者存在**继承关系的多个类**中  
* 重载**必须**要保证**被重载方法参数类型，个数，顺序任意有一项与原方法不一致**
```java
public class PrintDemo {
    public void write(int i) {
        System.out.println(i);
    }

    public void write(double f) {
        System.out.println(f);
    }

    public void write(String s) {
        System.out.println(s);
    }

    public static void main(String[] args) {
        PrintDemo pd = new PrintDemo();
        pd.write(11.02);
        pd.write("helloworld");
        pd.write(11);
    }
}
```

方法重载的目的是，功能类似的方法使用同一名字，更容易记住，因此调用起来更简单  

例如，String类提供了多个重载方法indexOf()，可以查找子串：
* `int indexOf(int ch)` 根据字符的Unicode码查找
* `int indexOf(String str)` 根据字符串查找
* `int indexOf(int ch, int fromIndex)` 根据字符查找，但指定起始位置
* `int indexOf(String str, int fromIndex)` 根据字符串查找，但指定起始位置

> 在重载中输入不同类型的参数会调用不同的方法，是多态的重要体现之一
