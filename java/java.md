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
        - [构造方法](#构造方法)
        - [标准类（Java Bean）](#标准类java-bean)
    - [java常用类](#java常用类)
        - [Scanner](#scanner)
        - [匿名对象](#匿名对象)
        - [random](#random)
        - [ArrayList](#arraylist)
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
            - [super与this](#super与this)
            - [super](#super)
            - [this](#this)
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
                - [静态代码块](#静态代码块)
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

### ArrayList
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
#### super与this
#### super
访问父类成员，引用当前对象的父类
#### this
指向自己的引用, 当方法的局部变量重名的时候，根据“就近原则”，优先使用局部变量。
如果需要访问本类中的成员变量，需要根据格式：

        this.成员变量

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
+ 静态变量：无论一个类实例化多少对象，静态变量也称为实例变量，局部变量不能被声明成为static方法。凡是本类的实例共享同一份。
+ 静态方法：静态方法不能使用非静态变量。静态方法从参数列表得到数据，然后计算这些数据。
成员方法可以访问成员变量；
成员方法可以访问静态变量；
静态方法可以访问静态变量；
静态方法**不能**直接反问成员变量。
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

