# JavaScript
## JavaScript内置对象
### 内置对象
JavaScript中的对象分为三种：
+ 自定义对象
+ 内置对象
+ 浏览器对象

### 查文档
[MDN](https://developer.mozilla.org/zh-CN/)

### Math对象
Math对象不是构造函数，它具有数学常数和函数的属性和方法。跟数学相关的运算（求绝对值，取整、最大值等）可以使用Math中的成员。
| Math 属性和方法 | 作用     |
| --------------- | -------- |
| Math.ceil()     | 向上取整 |
| Math.floor()    | 向下取整 |
| Math.PI         | 圆周率   |

## 数组操作
### 数组对象的创建
+ 字面量方式
+ new Array()
### 检测是否为数组
**istanceof**运算符，可以判断一个对象是否为某种类型

**Array.isArray()**用于判断一个对象是否为数组，isArray()是HTML5中提供的方法
### 新增
+ 数组.push()方法可以将一个或多个元素添加到数组的末尾，并返回该数组的长度
+ 数组.unshift()方法可以将一个或者多个元素添加到数组开头，并返回数组长度
### 删除
+ pop()删除数组最后一个元素，返回删除元素值
+ shift()删除数组第一个元素，并返回删除的第一个元素的值
### 数组的排序
| 方法名    | 说明                         | 是否修改原数组                     |
| --------- | ---------------------------- | ---------------------------------- |
| reverse() | 颠倒数组中元素的顺序，无参数 | 该方法会改变原来的数组，返回新数组 |
| sort()    | 对数组的元素进行排序         | 该方法会改变原来的数组，返回新数组 |
### 数组索引的方法
| 方法名        | 说明                           | 返回值                                   |
| ------------- | ------------------------------ | ---------------------------------------- |
| indexOf()     | 数组中查找给定元素的第一个索引 | 如果存在则返回索引号，如果不存在则返回-1 |
| lastIndexOf() | 在数组中的最后一个索引         | 如果存在则返回索引号，如果不存在则返回-1 |

### 遍历数组
```javascript
var arr = ['a', 'b', 'c']
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
```
## 函数
### 声明函数
```js
function 函数名(参数1, 参数2){
    //函数体代码
    return 返回值   //如果函数没有return， 返回的是undefined
}
```

### arguments的使用
不确定有多少个参数传递的时候，可以使用**arguments**来获取，在js中，arguments实际上它是当前函数的内置对象。所有函数都内置了一个arguments对象，arguments对象中**存储了传递的所有实参**。

**arguments展示形式是一个伪数组**，因此可以进行遍历。伪数组具有以下特点：
+ 具有length属性
+ 按索引方式存储数据
+ 不具有数组的push，pop等方法

### 匿名函数
```js
const fn = function(){...};
```

## API
+ DOM
+ BOM
### 通过class修改样式
元素.className='w banner'
### 通过classList修改样式
为了解决className容易覆盖以前的类名，我们可以通过classList方式追加和删除类名
语法:
```js
// 追加一个类
元素.classList.add('类名')
// 删除一个类
元素.classList.remove('类名')
//切换一个类
元素.classList.toggle('类名')
```

## 面向对象的程序设计
ECMAScript没有类的概念，ECMAScript-262把对象定义为：无序属性的集合，其属性可以包含基本值、对象或者函数。
### 理解对象
推荐语法，对象字面量语法：

    var person = {
        name: "zhangsan",
        age: 30,
        job: "Software Engineer",

        sayName: function() {
            alert(this.name();
        }
#### 属性类型
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

### 创建函数
#### 工厂模式
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
#### 构造函数模式
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

    var person1 = new Person("zhangsan", 20, "doctor");


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
#### 6.2.4 组合使用构造函数和原型模式
创建自定义类的最常见方法，就是组合使用构造函数模式与原型模式。构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。每个实例都会有自己的一份实例属性副本，但同时又共享着对方法的引用。

    function Person(name, age, job)){
        this.name = name;
        this.age = age;
        this.job = job;
        this.friends = ["xx", "yy"]
    }

    Person.prototype = {
        constructor: Person,
        sayName: function (){
            alert(this.name);
        }
    }

    var person1 = new Person("zhangsan", 20, "software engineer");
    var person2 = new Person("lisi", 28, "doctor");

### 6.3 继承
ECMAScript只支持实现继承，主要依靠原型链来实现。
#### 6.3.1 原型链
每个构造函数都有一个原型对象，原型对象包含一个指向构造函数的指针，每个实例都包含一个指向原型对象的内部指针。如果让原定对象等于另一个类型的实例，那么此时的原型对象将包含一个指向另一个原型的指针。
调用对象方法的步骤：搜索实例>搜索原型对象>沿着原型链向上搜索>Object.prototype
使用instanceof确认原型与实例直线的关系;或者使用isPrototypeOf()，只要原型链中出现过的原型，都会返回true

    alert(per1 instanceof Objcet); //true
    alert(per1 instanceof Person); //true
    alert(Objcet.prototype.isPrototypeOf(per1)); //true

原型链的问题：
+ 通过原型链继承，会造成引用类型属性被子类所有实例共享。
+ 不能向超类的构造函数中传递参数，所以实践中很少单独使用原型链。

#### 6.3.3 组合继承
组合继承（combination inheritance）也叫做伪经典继承，指的是将原型链和借用构造函数的技术组合到一块。使用原型链实现对属性原型属性和方法的继承，而借用构造函数实现实例属性的继承。

    function SuperType(name){
        this.name = name;
        this.colors = ["red", "blue", "green"]
    }

    Supertype.prototype.sayName = function(){
        alert(this.name);
    }

    function Subtype(name, age){
        Supertype.call(this.name);  //继承属性
    }
    Subtype.prototype = New Supertype();   //继承方法
    Subtype.prototype.constructor = Subtype;
    Subtype.prototype.sayAge = function(){
        alert(this.age);
    }

    var instance1 = new Subtype("zhangsan", 29);

### 7函数表达式
声明函数两种方法：1.函数式声明 2.函数表达式
函数式声明，重要特征“函数声明提升”，在执行代码之前会先读取函数声明。

    sayHi();
    function sayHi(){
        alert("balabala");
    }

函数表达式

    var sayHi = function() {
        alert("balabala");
    }

#### 7.1 递归
递归函数是在一个函数通过调用自身的情况下构成。

    function factorial(num){
        if (num <=1 ){
            return 1;
        }else{
            return  num * arguments.callee(num-1);   //arguments.callee是指向一个正在执行的函数的指针。
        }
    }



