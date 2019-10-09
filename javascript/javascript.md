## 6面向对象的程序设计
ECMAScript没有类的概念，ECMAScript-262把对象定义为：无序属性的集合，其属性可以包含基本值、对象或者函数。
### 6.1 理解对象
推荐语法，对象字面量语法：

    var person = {
        name: "zhangsan",
        age: 30,
        job: "Software Engineer",

        sayName: function() {
            alert(this.name();
        }
#### 6.1.1 属性类型
##### 数据属性
+ [[Configurable]] 是否能通过delete删除属性从而重新定义，默认true
+ [[Enumerable]] 是否通过for-in循环访问属性，默认true
+ [[Wirtable]] 能否修改属性的值，默认true
+ [[Vaule]] 包含这个属性的值
举例：

    var person = {
    Object.defineProperty(person, "name",{
        writeable: false,
        value: "zhangshan"
    }};

##### 访问器属性
访问器属性不包含数据值
+ [[Configurable]]: 能否通过delete删除属性从而重新定义属性，默认true
+ [[Enumerable]]: 能否通过for-in返回属性
+ [[Get]]: 读取属性时调用
+ [[Set]]: 写入属性时调用

### 6.2 创建函数
#### 6.2.1 工厂模式
工厂模式是软件工程领域一种广为人知的设计模式，这种模式抽象了创建具体兑现的过程。

    function createPerson(name, age, job):
        var o = new Object();    // 显示的穿件对象
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function() {
            alert(this.name);
        };
        return o;
    }

    var person1 = createPerson("zhangsan", 20, "doctor");

工厂模式虽然解决了创建多个相似对象的问题，却没有解决对象的识别问题（即怎样知道一个对象的类型）。
#### 6.2.2 构造函数模式
任何函数通过new操作符来调用，那它就可以作为构造函数。
按照惯例构造函数应该始终以一个大写字母开头，而非构造函数应该以一个小写字母开头。

    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
        this.sayName = function() {      //直接将属性和方法赋给this对象
            alert(this.name);
        }；
    }                                    //没有return语句

    var persion1 = new Person("zhangsan", 20, "doctor");


构造函数的主要问题每个方法都要在每个实例上创建一遍
#### 6.2.3 原型模式
每个函数都有prototype属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特性类型的所有实例共享的属性和方法。使用原乡对象的好处是可以让所有的实例共享它所包含的属性和方法。

    function Person() {
    }
    Person.prototype.name = "zhangsan";
    Person.prototype.age = 20;
    Person.Prototype.job = "doctor";
    Person.Prototype.sayname = function () {
        alert(this.name);
    };

    var person1 = new Person();
    person1.name();

无论什么时候，只要创建一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，这个属性指向函数的原型对象。所有原型对象都会获得一个constructor（构造函数）属性，这个属性是一个指向prototype属性所在的函数的指针。当构造函数创建一个新势力后，该实例的内部将包含一个指针，指向构造函数的原型对象。每个实例都包含一个内部属性，该属性仅仅指向了原型对象。
当代码杜旭摸个对象的某个属性时，都会执行一次搜索。从对象本身开始。如果在实例中找到了具有给定名字的属性，则返回该属性的值；如果没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性。
虽然可通过对象实例访问保存在原型中的值，却不能通过对象实例重写原型中的值。
