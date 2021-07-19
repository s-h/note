<!-- TOC -->

- [java](#java)
    - [数据类型](#数据类型)
        - [基本数据类型](#基本数据类型)
        - [引用数据类型](#引用数据类型)
    - [类型转换](#类型转换)
        - [强制类型转换](#强制类型转换)
    - [继承](#继承)
        - [继承的特性](#继承的特性)
        - [继承关键字](#继承关键字)
            - [extends](#extends)
            - [implements](#implements)
            - [super与this](#super与this)
    - [接口](#接口)
        - [接口的特性](#接口的特性)
        - [接口的实现](#接口的实现)
    - [修饰符](#修饰符)
        - [访问控制修饰符](#访问控制修饰符)
            - [defalut](#defalut)
            - [private](#private)
            - [public](#public)
            - [protected](#protected)
        - [非访问修饰符](#非访问修饰符)
            - [static](#static)
            - [final](#final)
            - [abstract](#abstract)

<!-- /TOC -->
# java
## 数据类型
### 基本数据类型
+ 整数型（byte short int long）
+ 浮点型（float double）
+ 字符型（char）
+ 布尔型（boolean)
### 引用数据类型
## 类型转换
### 强制类型转换
格式：范围小的类型 范围小的变量名 =(范围小的类型) 原本范围大的数据；

    int num = (int) 100L;

## 继承
### 继承的特性
+ 子类拥有父类非private的属性、方法
+ 子类可以拥有自己的属性和方法
+ 子类可以重写父类方法
### 继承关键字
#### extends
java类的继承属于大一继承，一个子类只能拥有一个父类
#### implements
继承多个接口

    public interface A {
        public void eat();
        public void sleep();
    }
    
    public interface B {
        public void show();
    }
    
    public class C implements A,B {
    }

#### super与this
+ super：访问父类成员，引用当前对象的父类
+ this：指向自己的引用
## 接口
接口（英文：Interface），在JAVA编程语言中是一个抽象类型，是抽象方法的集合，接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。
接口并不是类，编写接口的方式和类很相似，但是它们属于不同的概念。类描述对象的属性和方法。接口则包含类要实现的方法。
除非实现接口的类是抽象类，否则该类要定义接口中的所有方法。
接口无法被实例化，但是可以被实现。一个实现接口的类，必须实现接口内所描述的所有方法，否则就必须声明为抽象类。另外，在 Java 中，接口类型可用来声明一个变量，他们可以成为一个空指针，或是被绑定在一个以此接口实现的对象。
### 接口的特性
+ 接口是隐式抽象的，当声明一个接口时，不必使用abstract
+ 接口中的方法是隐式抽象的，不必使用abstract
+ 所有方法是public的


    /* 文件名 : Animal.java */
    interface Animal {
        public void eat();
        public void travel();
    }
    
### 接口的实现
当类实现接口的时候，类要实现接口中所有的方法。或者类声明为抽象类。

    ...implements 接口名称[, 其他接口名称, 其他接口名称..., ...] ...

## 修饰符
+ 访问修饰符
+ 非访问修饰符

### 访问控制修饰符
#### defalut
默认访问修饰符，不适用任何关键字
使用默认访问修饰符声明的变量和方法，对同一个包内的类是可见的。接口里的变量都隐式声明为public static final，而接口里的方法默认为public
#### private
私有访问修饰符，被声明为private的方法、变量和构造方法只能被所属的类访问，并且类和接口不能声明为private。
#### public
被声明为public的类、方法、构造方法和接口能被任何其他类访问
main()方法必须设置成public

    public static void main(String[] args){}
#### protected 
protected 可以修饰数据成员，构造方法，方法成员，不能修饰类（内部类除外）。
**子类与基类再同一包中**：被声明protected的变量、方法和构造器能被同一个包中的任何其他类访问
**子类与基类不在同一包中**：子类实例可以访问从基类集成而来的属性
### 非访问修饰符
#### static
+ 静态变量：无论一个类实例化多少对象，静态变量也称为实例变量，局部变量不能被声明成为static方法。
+ 静态方法：静态方法不能使用非静态变量。静态方法从参数列表得到数据，然后计算这些数据。
#### final
+ final变量：一旦赋值后，不能被重新赋值。
+ final方法：不能被子类重写。
+ final类：不能被继承。
#### abstract
+ 抽象类：不能用来实例化对象，声明抽象类的唯一目的是为了对该类进行扩充。abstract和final不能同时修饰类。包含一个或多个抽象方法，必须声明为抽象类。
+ 抽象方法：是一种没有任何实现的方法，具体实现由子类实现。


    public abstract class SuperClass{
        abstract void m(); //抽象方法
    }
    
    class SubClass extends SuperClass{
        //实现抽象方法
        void m(){
            .........
        }
    }

