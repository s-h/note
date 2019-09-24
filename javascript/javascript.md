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

    var person = {
    Object.defineProperty(person, "name",{
        writeable: false,
        value: "zhangshan"
    }};

##### 访问器属性
