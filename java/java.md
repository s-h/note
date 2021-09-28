<!-- TOC -->

- [java](#java)
    - [数据类型](#数据类型)
        - [基本数据类型](#基本数据类型)
        - [引用数据类型](#引用数据类型)
            - [数组(Array)](#数组array)
                - [创建数组](#创建数组)
                    - [动态初始化（指定长度）](#动态初始化指定长度)
                    - [静态初始化（指定内容）](#静态初始化指定内容)
                    - [获取数组长度](#获取数组长度)
                    - [java中的内存划分](#java中的内存划分)
    - [类型转换](#类型转换)
        - [强制类型转换](#强制类型转换)
    - [类](#类)
        - [使用对象类型作为参数方法](#使用对象类型作为参数方法)
        - [成员变量与局部变量](#成员变量与局部变量)
    - [封装](#封装)
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
java中单引号表示字符，双引号表示字符串
+ 布尔型（boolean)
### 引用数据类型
#### 数组(Array)
+ 数组是一种引用数据类型
+ 数组当中的多个数据，类型必须统一
+ 数组的长度在程序运行期间不可改变
##### 创建数组
###### 动态初始化（指定长度）
动态初始化格式：

    数据类型 [] 数组名称 = new 数据类型[数组长度];

###### 静态初始化（指定内容）
静态初始化没有指定长度，自动计算长度
静态初始化标准格式：

    数据类型 [] 数组名称 = new 数据类型[] {元素1, 元素2, ,,,}
    int[] myArray2 = new int[]{5, 10, 15};
    String[] myArray3 = new String[]{"hello", "world"};

静态初始化省略格式：

    数据类型 [] 数组名称 = {元素1, 元素2, ,,,}
    int[] myArray2 = {5, 10, 15};
    String[] myArray3 = {"hello", "world"};

###### 获取数组长度

    数组名称.length

###### java中的内存划分
+ 栈(Stack): 存放的都是方法中的局部变量，有作用域，一旦超出作用域，立刻从栈内存中消失
+ 堆(Heap): 凡是new出来的对象都在堆当中，堆内存里的东西都一个地址值（16进制），堆内存里的数据都有默认值：整数（0），浮点数(0.0)，字符（\u0000），布尔（false），引用类型（null）
+ 方法区（Method Area）：存储.class相关信息，包含方法的信息
+ 本地方法栈（Native Method Stack）：与操作系统相关。
+ 寄存器（pc Register）：与CPU相关。

## 类型转换
### 强制类型转换
格式：范围小的类型 范围小的变量名 =(范围小的类型) 原本范围大的数据；

    int num = (int) 100L;
## 类
成员变量（属性）：直接定义在类当中，在方法外。
成员方法（行为）：成员方法没有static关键字。
通常情况下，一个类并不能直接使用，需要根据一个类创建一个对象，才能使用：
导包：也就是指出需要使用的类，在什么位置：

    import 包名称.类名称；

创建：

    Student zhangsan = new Student();

使用：

    zhangsan.name; //成员变量
    zhangsan.eat(); //成员方法

### 使用对象类型作为参数方法
当一个对象作为参数，传递方法中时，实际上传递进去的是对象的**地址值**
### 成员变量与局部变量
+ 定义的位置不一样，局部变量在方法内部，成员变量在方法外部。
+ 作用范围不一样，局部变量在方法中，成员变量整个类。
+ 默认值不一样，局部变量没有默认值，成员变量有默认值。
+ 内存的位置不一样，局部变量位于栈内存，成员变量位于堆内存。
+ 生命周期，局部变量随着方法进栈而诞生，随着方法出栈而消失；成员变量随着对象创建而诞生，随着对象被垃圾回收而消失。

## 封装
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
一旦使用private对成员变量进行修复，本类中可以随意访问，但是，超出了本类就不能直接访问。
间接访问private成员变量，就是定义一堆Getter/Setter方法

    public class Persion {
        private int age;
        private boolean male;

        public void setAge(int num){   // Setter没有返回值
            age = num; 
        }

        public int getAge(){
            return age;
        }

        pubic void setMale(boolean b){
            male = b;
        }

        public boolean isMale(){    //布尔值获取使用is
            return male;
        }
    }}
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

