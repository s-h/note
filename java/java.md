<!-- TOC -->

- [java](#java)
    - [基本概念](#基本概念)
        - [JVM、JRE与JDK](#jvmjre与jdk)
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
    - [面向对象内存分析](#面向对象内存分析)
        - [java虚拟机内存模型](#java虚拟机内存模型)
            - [从属于线程的内存区域](#从属于线程的内存区域)
            - [堆（heap）](#堆heap)
            - [方法区（Method Area）](#方法区method-area)
            - [运行时常量池（Run-Time Const)](#运行时常量池run-time-const)
            - [直接内存（Direct Memory）](#直接内存direct-memory)
        - [程序运行时内存分析过程](#程序运行时内存分析过程)
            - [编写Person类](#编写person类)
        - [参数传值机制](#参数传值机制)
    - [垃圾回收机制](#垃圾回收机制)
    - [类](#类)
        - [使用对象类型作为参数方法](#使用对象类型作为参数方法)
        - [成员变量与局部变量](#成员变量与局部变量)
        - [构造方法](#构造方法)
        - [标准类（Java Bean）](#标准类java-bean)
        - [内部类](#内部类)
            - [成员内部类](#成员内部类)
            - [局部内部类（包含匿名内部类）](#局部内部类包含匿名内部类)
            - [匿名内部类](#匿名内部类)
    - [java常用类](#java常用类)
        - [Scanner](#scanner)
        - [匿名对象](#匿名对象)
        - [random](#random)
        - [List](#list)
            - [ArrayList](#arraylist)
                - [获取元素](#获取元素)
            - [LinkedList](#linkedlist)
                - [创建对象及添加元素方法](#创建对象及添加元素方法)
        - [String](#string)
            - [字符串常量池](#字符串常量池)
            - [equals](#equals)
            - [length](#length)
            - [concat(String str)](#concatstring-str)
            - [charAt(int index)](#charatint-index)
            - [indexof(String str)](#indexofstring-str)
            - [字符串截取](#字符串截取)
            - [字符串转换](#字符串转换)
            - [split](#split)
        - [Arrays](#arrays)
            - [Math](#math)
    - [封装](#封装)
    - [继承](#继承)
        - [继承的特性](#继承的特性)
        - [继承关键字](#继承关键字)
            - [extends](#extends)
            - [implements](#implements)
            - [重写override](#重写override)
            - [继承中的构造方法](#继承中的构造方法)
            - [super与this](#super与this)
            - [super](#super)
            - [this](#this)
    - [接口](#接口)
        - [接口中包含的内容](#接口中包含的内容)
            - [接口的常量](#接口的常量)
            - [接口的抽象方法](#接口的抽象方法)
            - [接口的默认方法](#接口的默认方法)
            - [接口的静态方法](#接口的静态方法)
            - [私有方法:](#私有方法)
        - [接口的特性](#接口的特性)
        - [接口的实现](#接口的实现)
        - [接口与类](#接口与类)
    - [修饰符](#修饰符)
        - [访问控制修饰符](#访问控制修饰符)
            - [修饰符总结](#修饰符总结)
            - [defalut](#defalut)
            - [private](#private)
            - [public](#public)
            - [protected](#protected)
        - [非访问修饰符](#非访问修饰符)
            - [static](#static)
                - [静态代码块](#静态代码块)
            - [final](#final)
            - [abstract](#abstract)
    - [多态（Polymorphism）](#多态polymorphism)
        - [向上转型](#向上转型)
        - [向下转型](#向下转型)
        - [instaceof](#instaceof)
    - [泛型](#泛型)
    - [常见的数据结构](#常见的数据结构)
        - [栈](#栈)
        - [队列](#队列)
        - [数组](#数组)
        - [链表](#链表)
            - [单向链表](#单向链表)
            - [双向链表](#双向链表)
        - [红黑树](#红黑树)

<!-- /TOC -->
# java
##  基本概念
### JVM、JRE与JDK
JVM (Java Virtual Machine)就是一个虚拟的用于执行bytecode字节码的"虚拟计算机"。
JRE (Java Runtime Environment)包含：Java虚拟机、库函数、运行Java应用程序必须的文件。
JDK (Java Development Kit)包含：JRE以及增加编译器和调试器等用于程序开发的文件。

如果只是运行java程序，只需要
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

    数据类型[] 数组名称 = new 数据类型[] {元素1, 元素2, ,,,}
    int[] myArray2 = new int[]{5, 10, 15};
    String[] myArray3 = new String[]{"hello", "world"};

静态初始化省略格式：

    数据类型[] 数组名称 = {元素1, 元素2, ,,,}
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
## 面向对象内存分析
### java虚拟机内存模型
#### 从属于线程的内存区域
JVM的内存划分中，有部分区域是线程私有的，有部分是属于整个JVM进程；我们将这部分归为一类。
+ 程序计数器（Program Counter Register），在JVM规范中，每个线程都有自己的程序计数器。这是一块较小的内存空间，存储当前线程正在执行的Java方法的JVM指令地址，即字节码的行号。如果正在执行Native方法，则这个计数器为空。
+ Java虚拟机栈（Java Virtal Machine Stack），同样也是属于线程私有区域，每个线程在创建的时候都会创建一个虚拟机栈，生命周期与线程一致，线程退出时，线程的虚拟机栈也回收。虚拟机栈内部保持一个个栈帧，每次方法调用都会进行压栈。该区域存储着局部变量表，编译时可知的各种基本类型数据、对象引用、方法出口等信息。
+ 本地方法栈（Native Method Stack）与虚拟机栈类似，本地方法栈是在调用本地方法时使用的栈，每个线程都有一个本地方法栈。
#### 堆（heap）
堆（Heap），几乎所有创建的 Java对象实例，都是被直接分配到堆上的。堆被所有的线程所共享，在堆上的区域，会被来及回收器做进一步划分，例如新生代、老年代的划分。Java虚拟机在启动的时候，可以使用“Xmx”之类的参数指定堆区域的大小。
#### 方法区（Method Area）
方法区与堆一样，也是所有线程所共享，存储被虚拟机加载的元（Meta）数据，包括类信息、常量、静态变量、即时编译器编译后的代码等数据。

方法区是一种java虚拟机的规范。由于方法区存储的数据和堆中存储的数据一致，实质上也是堆，因此，在不同的jdk版本中方法区的实现方式不一样。

JDK7以前，方法区就是堆中的“永久代”；JDK7以后开始去“永久代”，把静态变量、字符串常量池都挪到了堆内存中; JDK8以后，“永久代”不存在了，存储的类信息、编译后的代码数据等已经移动到了MetaSpace（元空间）中，元空间并没有处于堆内存上，而是（直接内存）直接占用的本地内存（NativeMemory）。 
#### 运行时常量池（Run-Time Const)
方法区的一部分。常量池主要有两大类常量：
+ 字面量（Literal），如文本字符串、final常量值。
+ 符号引用，存放了与编译相关的一些常量。
#### 直接内存（Direct Memory）
直接内存并不属于Java规范规定的属于Java虚拟机运行数据区的一部分。Java的NIO可以直接用Native方法直接在java堆外分配内存，使用DirectByteBuffer对象作为这个堆外内存的引用
### 程序运行时内存分析过程
便于理解，Java虚拟机的内存可以简单的分为三个内存区域：虚拟机栈stack、堆heap、方法区method area。

**栈**的特点如下：
+ 栈描述是方法执行的内存模型。每个方法被调用都会创建一个栈帧（存储局部变量、操作数、方法出口等）
+ JVM为每个线程创建一个栈，用于存放该线程执行方法的信息（实际参数、局部变量等）
+ 栈属于线程私有，不能实现线程间的共享
+ “先进后出，后进先出”
+ 栈是系统自动分配的一块连续内存空间，速度快。

**堆**的特点：
+ 堆用于村粗创建好的对象和数组
+ JVM只有一个堆，被所有线程共享
+ 堆是一个不连续的内存空间，分配灵活，速度慢。

**方法区（静态区）**特点如下：
+ 方法区是JAVA虚拟机规范，可以有不同的实现。
#### 编写Person类

    // 编写Person类
    public class Persion{
        String name;
        int age;
        public void show(){
            System.out.println("姓名: " + name + ",年龄: " + age);
        }
    }

    // 创建Person类对象并使用
    public class TestPerson {
        public static void main(String[] args){
            // 创建p1对象
            Person p1 = new Person();
            p1.age = 24;
            p1.name = "zhangsan";
            p1.show();
            // 创建p2对象
            Persion p2 = new Person();
            p2.age = 34;

        }
    }

![过程分析](java_img/java%E5%86%85%E5%AD%98.png)
### 参数传值机制

## 垃圾回收机制
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

### 构造方法
构造方法的调用通过new关键字
1. 构造方法的名称必须和所在的类名称完全一样，大小写一样；
2. 构造方法不能return，不能写void
3. 如果没有写任何构造方法，编写器默认添加一个构造方法，方法体为空。

    public class Student(){

        public Student(){
            //方法体
        }
    }
### 标准类（Java Bean）
一个标准的类通常要拥有下面四个组成部分
1. 所有的成员变量都要使用private关键字修饰
2. 为每一个成员变量编写一对儿Getter/Setter方法
3. 编写一个无参数的构造方法
4. 编写一个全参数的构造方法

一个标准的的类也叫做Java Bean

    public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }


    public void setAge(int age) {
        this.age = age;
    }
}
### 内部类
#### 成员内部类
内用外随意访问；外用内需要内部类对象

    修饰符 class 外部类名称{
        修饰符 class 内部类名称{

        }

    }

使用成员内部类有两种方法：
1.间接方法：在外部类的方法中使用内部类；然后main只是调用外部类的方法；
2.直接方法：类名称.内部类名称 对象名 = new 类名称().new 内部类名称();

内部类的同名变量访问
如果重名现象，那么内部类访问外部类成员变量：外部类名称.this.变量名称

    public class Outer{
        int num = 10;

        public class Innter{
            int num = 20;
            
            public void methodInner(){
                int num = 30;
                Sout(num); //内部类方法的局部变量；
                Sout(this.num); //内部类的成员变量；
                Sout(Outer.this.num); //外部类的成员变量；
            }

        }
    }

#### 局部内部类（包含匿名内部类）

    修饰符 class 外部类名称{
        修饰符 返回值类型 外部方法名称(参数列表){
            class 局部类名称 {
                ...
            }
        }
    }

局部内部类，如果希望访问所在方法的局部变量，那么这个局部变量必须是**有效final**的
备注：从Java8+开始，只要局部变量事实不变，那么final关键字可以省略
原因：
1. new出来的对象在堆内存当中
2. 局部变量是跟着方法走的，在栈内存当中
3. 方法运行结束后，立刻出栈，局部变量就会立刻消失
4. 但是new出来的对象会在堆当中持续存在，直到垃圾回收消失

#### 匿名内部类
如果接口的实现类（或者父类的子类）只需要使用一次，那么这种情况就可以使用**匿名内部类**
匿名内部类的定义格式：
接口名称 对象名 = new 接口名称(){
    //覆盖重写所有抽象方法
}

## java常用类
### Scanner

    Scanner sc = new Scanner(System.in);
    // 获取键盘标准输入
    int num = sc.nextInt();
    String st = sc.next();
### 匿名对象
匿名对象就是只有右边的对象，没有左边的名字和赋值

    new 类名称();
    int num = new Scanner(system.in).nextInt();

匿名对象只能使用唯一的一次，如果确定有一个对象只需要使用唯一的一次，就可以使用匿名对象。
### random

    Random r = new Random(); 
    r.nextInt();    //返回int范围内随机数
    r.nextInt(3);   //范围0~2

### List
#### ArrayList
java.util.ArrayList 是大小可变的数组的实现，存储在内的数据称为元素。此类提供一些方法来操作内部存储的元素。ArrayList中可不断添加元素，其大小也自动增长。
**泛型**也就是集合中的所有元素都是同一个的类型，泛型只能是**引用类型**

    //创建一个ArrayList集合，集合的名称是list，里面装的全是String类型
    ArrayList<String> list = new ArrayList<>();
    // 添加
    list.add("倚天剑");
    // 获取元素 返回<E>元素
    String name = list.get(0);
    // 删除元素 返回<E>元素
    String whoRmoved = list.remove(0);
    // 获取集合元素个数
    int size = list.size();

如果希望向集合里存放基本类型，必须使用基本类型对应的“包装类”：
+ byte --> Byte
+ short --> Short 
+ int --> Integer
+ long --> Long
+ float --> Float
+ doubel --> Double
+ char --> Character
+ boolean --> Boolean

    ArrayList<Integer> list = ArrayList<>();
    list.add(100);


##### 获取元素

    List<String> list = new ArrayList<>();
    list.add("倚天剑");
    list.add("屠龙刀");
    System.out.println(list);

    //使用普通的for循环
    for (int i = 0; i < list.size(); i++) {
        System.out.println(list.get(i));
    }
    //使用迭代器
    Iterator<String> it = list.iterator();
    while(it.hasNext()){
        String s = it.next();
        System.out.println(s);
    }
    // 增强for循环
    for(String s : list){
        System.out.println(s);
    }

####  LinkedList
list接口的链表列表实现
##### 创建对象及添加元素方法

    //创建Linked集合，不能使用多态
    LinkedList<String> linked = new LinkedList<>();

    //使用add的方法添加元素
    linked.add("火焰刀");
    linked.add("玄铁剑");

    //使用addFirst将元素插入列表的开头
    //等效push方法
    linked.addFirst("倚天剑");
    linked.push("屠龙刀");

    //addLast将指定的元素添加到集合末尾
    //等效add方法
    linked.addLast("碧血剑");
    linked.add("圣火令");

    
### String
字符串的特点：
+ 字符串的内容永不可变
+ 字符串可以共享使用
+ 字符串效果上相当于是char[]字符数组，底层原理是byte[]字节数组。

三种构造方法：
public String() 创建一个空白字符串，不含有任何内容。
public String(char[] array) 根据字符数组的内容，创建对应的字符串
public String(byte[] array) 根据字节数组的内容，创建对应的字符串

    // 使用空参构造
    String str1 = new String();

    // 根据字符串创建字符串
    char[] charArray = {'A', 'B', 'C'};
    String str2 = new String(charArray);

    // 根据字节数组创建字符串
    byte[] byteArray = {97, 98, 99};
    String str3 = new String(byteArray);

    // 直接创建
    String str4 = "hello";

#### 字符串常量池
**字符串常量池**程序中直接写上的双引号字符串，就在字符串常量池中
对于基本类型 == 比较的是数值
对于引用类型 == 比较的是地址值

    // 字符串常量池
    String str4 = "abc";
    String str5 = "abc";

    char [] charArray2 = {'a', 'b', 'c'};
    String str6 = new String(charArray2);

    System.out.println(str4 == str5);  //true
    System.out.println(str5 == str6);  //false
    System.out.println(str4 == str6);  //false

#### equals
pulibc boolean equals(Object obj) ,参数可以是任何对象，只有参数是一个字符串并且内容相同的才会给true否则返回false

    str4.equals(str6);
    "abc".equals(str4);

#### length
获取字符串含有的字符个数，拿到字符串长度

#### concat(String str)
将当前字符串和参数拼接成为返回值新的字符串

#### charAt(int index)
获取指定索引位置的单个字符

#### indexof(String str)
查找参数字符串在本字符串当中首次出现的索引位置，如果没有返回-1

#### 字符串截取
public String substring(int index) 截取从参数位置一直到字符串结尾，返回新字符串
public String substring(int begin, int end) 截取从begin开始，一直end结束，中间的字符串

#### 字符串转换
public char[] toCharArray()   将当前字符串拆分成为字符数组作为返回值。
public byte[] getBytes()      获取当前字符串地城的字节数组
public String replace(CharSequence oldString, CharSequence new String) 将所有出现的老字符串替换为新的字符串，返回替换之后的结果新字符串。
#### split
public String[] split(String regex) 按照规则，将字符串切分成若干部分，返回数组

### Arrays
java.util.Arrays是一个与数组相关的工具类，里面提供了大量静态方法，用来实现数组常见的操作。
public static String toString(数组) 将参数数组变为数组
public static void sort(数组) 按照默认升序对数组进行排序

#### Math
java.util.Math 数学相关工具类
public static double abs(double)  获取绝对值
public static double ceil(double) 向上取整
public static double floor(double) 向下取整
public static long round(double num) 四舍五入

## 封装
## 继承
继承主要解决的问题就是共性抽取

    public class 父类名称 {
        ...
    }

    public class 子类名称 extends 父类名称 {

    }

java语言是**单继承**的，一个类的直接父类只能有一个，可以**多级继承**
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
#### 重写override
方法覆盖重写的注意事项
1.必须保证父子类之间的方法名称相同，参数列表也相同。
@Override 写在方法前面，用来检测是不是有效的正确覆盖重写。
2.子类方法的返回值必须**小于等于**父类方法的返回值范围。
前提：java.lang.Object类是所有类的公共最高父类
3.子类方法的权限必须**大于等于**父类方法的权限修饰符。
public > protected > (default) > private
#### 继承中的构造方法
继承中的构造方法的访问特点
1.子类构造方法当中有一个默认隐含的“super()”
2.可以super调用父类重载构造
3.super的父类构造调用
#### super与this
#### super
访问父类成员，引用当前对象的父类
super的使用方法有三种：
1.在子类的成员方法中，访问父类的成员变量
2.在子类的成员方法中，访问父类的的成员方法
3.在子类的构造方法中，访问父类的构造方法
#### this
指向自己的引用, 当方法的局部变量重名的时候，根据“就近原则”，优先使用局部变量。
如果需要访问本类中的成员变量，需要根据格式：

        this.成员变量

1.在本类的方法中，访问本类的成员变量

    public class DemoThis {
        int num = 20；

        public void showNum(){
            int num = 10;
            System.out.println(num);    // 10, 局部变量
            System.out.println(num);    // 20, 成员变量
        }
    }

2.在本类的成员方法中访问另一个成员方法。
3.在本类的构造方法中访问另一个构造方法(必须是构造方法的第一个语句)。


## 接口
接口（英文：Interface），在JAVA编程语言中是一个抽象类型，是抽象方法的集合，接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。
接口并不是类，编写接口的方式和类很相似，但是它们属于不同的概念。类描述对象的属性和方法。接口则包含类要实现的方法。
除非实现接口的类是抽象类，否则该类要定义接口中的所有方法。
接口无法被实例化，但是可以被实现。一个实现接口的类，必须实现接口内所描述的所有方法，否则就必须声明为抽象类。另外，在 Java 中，接口类型可用来声明一个变量，他们可以成为一个空指针，或是被绑定在一个以此接口实现的对象。
### 接口中包含的内容
#### 接口的常量
java7及以上
public static final 三个关键字可以省略，默认已添加；接口当中的常量必须进行赋值。
建议常量大写下划线分割

    public static final 数据名称 常量名称 = 数据值;
#### 接口的抽象方法  
java7及以上
    
    public abstract 返回值类型 方法名称(参数列表);

#### 接口的默认方法
从java8开始，接口里允许定义默认方法

    public default 返回值类型 方法名称(参数列表){
        方法体
    }

#### 接口的静态方法 
从java8开始

    pubilc static 返回值类型 方法名称(参数列表){方法体};

不能通过接口实现类对象来调用接口当中的静态方法；应该通过接口名称调用其中的静态方法：接口名称.静态方法(参数)

#### 私有方法: 
从java9开始
普通私有方法、静态私有方法
### 接口的特性
+ 接口是隐式抽象的，当声明一个接口时，不必使用abstract
+ 接口中的方法是隐式抽象的，不必使用abstract
+ 所有方法是public的
(public 和 abstract可以省略)

    /* 文件名 : Animal.java */
    interface Animal {
        public void eat();
        public void travel();
    }
    
### 接口的实现
接口不能直接使用，必须有一个“实现类”来“实现”接口；接口的实现类必须覆盖重写接口中所有的抽象方法，如果实现类并没有覆盖重写所有抽象方法，则该类必须是抽象类
当类实现接口的时候，类要实现接口中所有的方法。或者类声明为抽象类。
使用接口时需要注意：
1.接口没有静态代码块或构造方法。
2.一个类的直接父类是唯一的，但是一个类可以同时实现多个接口
3.如果实现类所实现的多个接口中，存在重复的抽象方法，那么只需要覆盖重写一次即可
4.实现类一定要对冲突的默认方法进行重写。
5.如果实现类没有覆盖重写所有的抽象方法，那么实现类一定是一个抽象类。
6.一个类直接父类的方法和接口的默认方法出现了冲突，优先使用父类当中的方法。

    ...implements 接口名称[, 其他接口名称, 其他接口名称..., ...] ...

### 接口与类
1.类与类之间是单继承的。直接父类只有一个
2.类与接口之间是多实现的。
3.接口与接口之间是多继承的。

## 修饰符
+ 访问修饰符
+ 非访问修饰符

### 访问控制修饰符
#### 修饰符总结
Java有四种权限：
| 位置         | public | protected | (default) | private |
| ------------ | ------ | --------- | --------- | ------- |
| 不同包子类   | YES    | YES       | NO        | NO      |
| 不同包非子类 | YES    | NO        | NO        | NO      |
| 同一个包     | YES    | YES       | YES       | NO      |
| 同一个类     | YES    | YES       | YES       | YES     |
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
+ 静态变量：无论一个类实例化多少对象，静态变量也称为实例变量，局部变量不能被声明成为static方法。凡是本类的实例共享同一份。
+ 静态方法：静态方法不能使用非静态变量。静态方法从参数列表得到数据，然后计算这些数据。
成员方法可以访问成员变量；
成员方法可以访问静态变量；
静态方法可以访问静态变量；
静态方法**不能**直接访问成员变量。
静态方法不能使用this关键字。
##### 静态代码块
当第一次使用本类时，静态代码块执行唯一的一次。
静态内容总是优于非静态，所以静态代码块比构造先执行。
典型用途：用来一次性的对静态成员变量进行赋值。

    public class 类名称{
        static {
            // 静态代码块
        }
    }

#### final
+ final变量：一旦赋值后，不能被重新赋值；修饰成员变量要么直接赋值，要么使用构造方法赋值。
+ final方法：不能被子类重写。
+ final类：不能被继承。
注意：对于引用类型不可变指的是引用的地址值不可变
#### abstract
+ 抽象类：不能用来实例化对象，声明抽象类的唯一目的是为了对该类进行扩充。abstract和final不能同时修饰类。包含一个或多个抽象方法，必须声明为抽象类。
+ 抽象方法：是一种没有任何实现的方法，具体实现由子类实现。抽象方法所在的类**必须**是抽象类。
一个抽象类不一定含有抽象方法，只有保证含有抽象方法的类是抽象类即可


    public abstract class SuperClass{
        abstract void m(); //抽象方法
    }
    
    class SubClass extends SuperClass{
        //实现抽象方法
        @Override
        void m(){
            .........
        }
    }

如何使用抽象类：
1.不能直接创建new抽象对象
2.必须用一个子类继承抽象类
3.子类必须覆盖重写抽象父类中的所有抽象方法

## 多态（Polymorphism）
代码当中体现多态性，其实就一句话：父类引用指向子类对象

    父类名称 对象名 = new 子类名称();
    接口名称 对象名 = new 实现类名称();

访问成员变量的两种方法：
1.直接通过对象名称访问成员变量：看等号左边是谁，优先用谁，没有则向上找。
2.间接通过成员方法访问成员变量，看该方法属于谁，优先用谁，没有则向上找。

在多态的代码当中，成员方法的访问规则是：
看new的是谁，就优先用谁，没有则向上找。

口诀：
成员方法：编译看左边，运行看右边。
成员变量: 编译看左边，运行看左边。

    Fu obj = new Zi(); //多态
    obj.method(); 父子都有优先用子
    obj.methodFu(); 子类没有，父类有，向上找父类

    // 编译看左边，Fu当中没有methodZi方法，所以编译报错
    obj.methodZi(); // 编译报错

### 向上转型
对象的向上转型，其实就是多态的写法

    格式： 父类名称 对象名 = new 子类名称();                 Animal animal = new Cat();
    含义： 右侧创建一个子类对象，把它当做父类来看待使用        创建一只猫，当做动物来使用

注意：**向上转型一定是安全的** 从小范围转向了大范围，从小范围的猫，向上转换成为更大范围的动物。
### 向下转型
对象的向下转型其实是一个还原的动作

    格式： 子类名称 对象名 = （子类名称)父类对象；             Cat cat = (Cat) animal;
    含义： 将父类对象，还原成为本来的子类对象。

### instaceof
判断前面的对象能不能当做后面类的实例

    对象 instanceof 类名称

## 泛型
**泛型**是一种未知的数据类型，当我们不知道使用什么数据类型的时候可以使用泛型，
泛型也可以看做变量，用来接收数据类型

    E e: Element 元素
    T t: Type 类型

ArraryList<E>
创建集合对象使用泛型的好处：
1. 避免了数据类型转换的麻烦，存储的是什么类型，取出的就是什么类型
2. 把运行期的异常提升到编译期的异常

## 常见的数据结构
### 栈
栈（stack）又称堆栈，它是运算受限的线性表，其限制是仅允许在标的一端进行插入和删除操作，不允许在其他位置进行添加、查找、删除等操作。
栈的特点：先进后出
### 队列
队列的特点：先进先出
### 数组
数组（Array）是有序的元素序列，数组是在内存中开辟一段连续的空间，并在空间存放元素。
数组的特点：
1. 查找元素快：通过索引，可以快速存取指定位置的元素
2. 增删元素慢：
    1. 指定索引位置增加元素；需要创建一个新的数组，将指定新元素存储在指定索引位置，再把原数组元素根据索引，复制到新数组对应的索引位置
### 链表
链表（linked list）：由一系列结点node（链表中的每一个元素成为结点）组成，结点可以在运动时动态生成。每个结点包含两部分：一部分是存储元素的数据域，另一个是存储下一个结点地址的指针域。链表分为单向链表、双向链表。
链表的特点：
1. 查找元素慢
2. 增删元素快
#### 单向链表
链表中只有一条链子，不能保证元素的顺序
#### 双向链表
链表中有两条链子，有一条专门记录元素的顺序，是一个有序的集合
### 红黑树
