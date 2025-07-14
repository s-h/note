<!-- vscode-markdown-toc -->
* 1. [web标准的构成](#web)
	* 1.1. [结构](#)
	* 1.2. [表现](#-1)
	* 1.3. [行为](#-1)
* 2. [css三大特性](#css)
	* 2.1. [层叠性](#-1)
	* 2.2. [继承性](#-1)
		* 2.2.1. [行高的继承](#-1)
	* 2.3. [优先级](#-1)
* 3. [选择器](#-1)
	* 3.1. [基础选择器](#-1)
		* 3.1.1. [标签选择器](#-1)
		* 3.1.2. [类选择器](#-1)
		* 3.1.3. [id选择器](#id)
		* 3.1.4. [通配符选择器](#-1)
	* 3.2. [复合选择器](#-1)
		* 3.2.1. [后代选择器](#-1)
		* 3.2.2. [子元素选择器](#-1)
		* 3.2.3. [并集选择器](#-1)
		* 3.2.4. [伪类选择器](#-1)
	* 3.3. [选择器优先级](#-1)
* 4. [字体属性](#-1)
	* 4.1. [字体粗细](#-1)
	* 4.2. [字体符合属性](#-1)
* 5. [文本属性](#-1)
* 6. [背景属性](#-1)
	* 6.1. [背景颜色](#-1)
	* 6.2. [背景图片](#-1)
	* 6.3. [背景平铺](#-1)
	* 6.4. [背景图片位置](#-1)
	* 6.5. [背景图像固定（背景附着）](#-1)
	* 6.6. [背景属性复合属性](#-1)
	* 6.7. [背景颜色半透明](#-1)
* 7. [css引入方式](#css-1)
	* 7.1. [内部样式表](#-1)
	* 7.2. [外部样式表](#-1)
* 8. [Emmet语法](#Emmet)
	* 8.1. [使用Emmet快速生成html](#Emmethtml)
	* 8.2. [使用Emmet快速生成css](#Emmetcss)
	* 8.3. [元素的显示模式](#-1)
		* 8.3.1. [块元素](#-1)
		* 8.3.2. [行内元素](#-1)
		* 8.3.3. [行内块元素](#-1)
		* 8.3.4. [元素显示模式转换](#-1)
* 9. [盒子模型](#-1)
	* 9.1. [盒子模型的组成](#-1)
		* 9.1.1. [border](#border)
		* 9.1.2. [box-shadow盒子阴影](#box-shadow)
		* 9.1.3. [padding](#padding)
		* 9.1.4. [margin](#margin)
* 10. [传统网页布局的三种方式](#-1)
	* 10.1. [普通流(标准流)](#-1)
	* 10.2. [浮动](#-1)
		* 10.2.1. [浮动的特性](#-1)
		* 10.2.2. [浮动元素和标准流父级搭配使用](#-1)
		* 10.2.3. [清除浮动](#-1)
	* 10.3. [定位](#-1)
		* 10.3.1. [定位模式](#-1)
		* 10.3.2. [边偏移](#-1)
* 11. [css书写顺序](#css-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

# web标准

##  1. <a name='web'></a>web标准的构成

###  1.1. <a name=''></a>结构
结构（Structure）用于对网页元素进行整理和分类：html

###  1.2. <a name='-1'></a>表现
表现（Presentation）用于表现设置网页元素的版式、颜色、大小等外观样式：css

###  1.3. <a name='-1'></a>行为
行为（Behavior）是指网页的定义及交互的编写：JavaScript

# html

# css
CSS是层叠样式（Cascading Style Sheets）的简称

##  2. <a name='css'></a>css三大特性

###  2.1. <a name='-1'></a>层叠性
相同选择器给设置相同的样式，此时一个样式就会**覆盖（层叠）**另一个冲突的样式。层叠性主要解决样式冲突的问题
层叠性原则：
+ 样式冲突，遵循的原则是就近原则，哪个样式离结构近，就执行哪个样式
+ 样式不冲突，不会层叠

###  2.2. <a name='-1'></a>继承性
子标签会继承父标签的某些样式，如文本的颜色和字号。

####  2.2.1. <a name='-1'></a>行高的继承

    body {
        font: 12px/1.5 Microsoft YaHei;
    }
    div {
        /* 子元素继承了父元素body的行高1.5 */
        /* 1.5b表示当前元素文字大小的1.5倍，此例中行高为1.5*14px=21px */
        font-size: 14px;
    }

+ 行高可以跟单位也可以不跟单位

###  2.3. <a name='-1'></a>优先级
+ 选择器相同，则执行层叠性
+ 选择器不相同，则更具[选择器优先级](#选择器优先级)执行

##  3. <a name='-1'></a>选择器
选择器分为基础选择器和复合选择器
基础选择器包含：标签选择器、类选择器、id选择器和通配符选择器

###  3.1. <a name='-1'></a>基础选择器

####  3.1.1. <a name='-1'></a>标签选择器

    标签名 {
        属性 
    }

####  3.1.2. <a name='-1'></a>类选择器

    .类 {
        属性
    }

##### 类命名规则
+ 头 header
+ 内容 content/container
+ 尾 footer
+ 导航 nav/navbar
+ 侧栏 siderbar
+ 栏目 column
+ 页面外围控制整体布局宽度 wrapper
+ 左中右 left center right
+ 登录条 loginbar
+ 广告 banner
+ 页面主体 main
+ 热点 hot
+ 子导航 subnav
+ 菜单 menu
+ 子菜单 submenu
+ 搜索 search

####  3.1.3. <a name='id'></a>id选择器
使用#号定义 ,使用id进行调用，只能调用一次，如果有标签调用了一次，其他标签不能进行调用

    #id{
        属性
    }

####  3.1.4. <a name='-1'></a>通配符选择器
在css中，通配符选择器使用“*”定义，它表示选取页面中所有元素
```css
* {
    属性1: 属性值;
    ...
}
通配符选择器不需要调用，自动会给所有元素使用样式
特殊情况才使用，如清除所有元素标签的内外边距
* {
    margin: 0
    padding: 0
}
```

###  3.2. <a name='-1'></a>复合选择器

####  3.2.1. <a name='-1'></a>后代选择器
后代选择器又称为包含选择器，可以选择父元素里面的子元素，修改的是子元素及后代元素的样式。
例如：选择ol中的li，修改的是li的样式

    ol li {
        属性
    }

选择nav类li中的a

    .nav li a {
        属性
    }    

####  3.2.2. <a name='-1'></a>子元素选择器
子元素选择器只能为某元素的最近一级子元素。简单理解就是子元素。
语法：元素>子元素   

    <div class="nav">
        <a href="#">子元素选择器只选择我</a>
        <p>
            <a href="#">后代选择器也会选择我</a>
        </p>
    </div>

    .nav>a {}   子元素选择器只选择第一个a
    .nav a {}   后代选择器两个a都选择

####  3.2.3. <a name='-1'></a>并集选择器
并集选择器可以选择多组标签，同时为他们定义相同的样式。通常用于集体声明。
语法：标签1， 标签2 

####  3.2.4. <a name='-1'></a>伪类选择器
伪类选择器用于向某些选择器添加特殊效果，比如给链接添加特殊效果，或者选择第1个，第n个元素。伪类选择器书写最大的特点用冒号：
比如：

    :hover
    :first-child

##### 链接伪类选择器
注意事项:按照lvha顺序书写，a链接在浏览器中有默认样式，实际开发中需要给链接指定样式。
1. a:link 选择所有未被访问的链接
2. a:visited 选择所有已被访问的链接
3. a:hover 选择鼠标指针位于其上的链接
4. a:active 选择活动链接（鼠标按下未弹起的链接）

    /* 1.未被访问的链接颜色、取消下划线 */
    a:link {
        color: #333;
        text-decoration: none;       
    }
    /* 2.当链接访问过的 */
    a:visited {
        color: orange;
    }
    /* 3.鼠标经过的链接*/
    a:hover {
        color: blue;
    } 
    /* 4.活用的链接 */
    a:avitve {
        color：green;
    }

##### focus伪类选择器
:focus伪类选择器用于获得焦点的表单元素

    /* 把光标所在的input元素选取出来 */
    input: focus {
        属性
    }

###  3.3. <a name='-1'></a>选择器优先级
!important > 行内样式 > ID > 类(伪类、属性) > 标签
不推荐使用important优先级 

##  4. <a name='-1'></a>字体属性
+ font-size 字号
+ font-family
+ font-weight 字体粗细
+ font-style italic/normal
+ font 字体连写

标题font-size比较特殊，需要单独指定文字大小
###  4.1. <a name='-1'></a>字体粗细
| 属性值  | 描述                           |
| ------- | ------------------------------ |
| 100-900 | 400 等于 normal，700 等于 bold |
| bold    | 加粗                           |
| normal  | 默认值                         |

加粗标签可以使用该属性不加粗
###  4.2. <a name='-1'></a>字体符合属性
```css
body {
    font: font-style font-weight font-size/line-height font-family;
}
```


##  5. <a name='-1'></a>文本属性
+ color 文本颜色 通常十六进制
+ text-align 文本对齐
+ text-indent 文本缩进
+ text-decoration 文本修饰 underline/none
+ line-height 行高
tips:
文字的行高等于盒子的高度，实现**当行文字垂直居中**，行高小于盒子的高度：文字偏上、行高大于盒子的高度：文字偏下
首行缩进两个字符
```css
p {
    text-indent 2em;
}
```

##  6. <a name='-1'></a>背景属性

###  6.1. <a name='-1'></a>背景颜色
background-color: transparent | color

###  6.2. <a name='-1'></a>背景图片
background-image: none | url(url)
属性描述了元素的背景图片。实际开发中常见logo或者装饰性的小图片或者超大的背景图片，优点是非常灵活便于控制位置。

###  6.3. <a name='-1'></a>背景平铺
background-repeat: repeat | no-repeat | repeat-x | repeat-y

###  6.4. <a name='-1'></a>背景图片位置
利用backgyound-position属性可以改变图片在背景中的位置
background-position: x y;
参数代表的意思是：x坐标和y坐标。可以使用方位名词和精确单位
参数值：
length 百分数|由浮点数字和单位标识符组成的长度值
position top | center | left | center | right

###  6.5. <a name='-1'></a>背景图像固定（背景附着）
background-attachment: scroll | fixed
属性设置背景图像是否固定或者随着页面的其余部分滚动

###  6.6. <a name='-1'></a>背景属性复合属性
顺序没有要求，一般写作：
background: 背景颜色 背景图片地址 背景平铺 背景图像滚动 背景图片位置;

###  6.7. <a name='-1'></a>背景颜色半透明
background: rgba(0, 0, 0, 0.3);
最后一个参数是alpha透明度，取值范围0~1之间


##  7. <a name='css-1'></a>css引入方式
+ 行内样式表
+ 内部样式表
+ 外部样式表
###  7.1. <a name='-1'></a>内部样式表
理论上style标签可以放到html文档的任意位置，一般放到head里
```html
<head>
    <style>
        div {
            color: red;
        } 
    </style>
</head>
```

###  7.2. <a name='-1'></a>外部样式表
1. 新建.css外部样式表
2. <link rel"stylesheet" href="css文件路径">

##  8. <a name='Emmet'></a>Emmet语法
它使用缩写，来提高html/css的书写速度

###  8.1. <a name='Emmethtml'></a>使用Emmet快速生成html
输入完成后需要使用tab键进行自动补全
1. 生成标签，直接输入标签
2. 生成相同的标签，标签*数字
3. 如果有父子级的标签，可以使用>，比如：ul>li
4. 如果有兄弟级别的标签，可以使用+，比如：div+p
5. 如果生成有类名或者id名字的，直接写.foo或者#bar，默认为div，可以使用：标签.id、标签#类名
6. 如果生成的div类名是有顺序的，可以使用自增符号$，例如.foo$*5生成class为foo1-foo5的div
7. 如果想要在生成标签中写内容可以使用{}，比如div{div中的内容}

###  8.2. <a name='Emmetcss'></a>使用Emmet快速生成css
css基本采取简写的形式使用tab进行补全
1. 比如w200 -> width: 200px;
2. 比如lh26 -> line-height: 26px;
3. 比如tac -> text-align: center;

###  8.3. <a name='-1'></a>元素的显示模式

####  8.3.1. <a name='-1'></a>块元素
常见的块元素：<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>，其中div是典型的块级元素
块元素的特点：
1. 独占一行
2. 高度、宽度、外边距及内边距都可以控制
3. 宽度默认是容器 （父级宽度）的100%
4. 是一个容器及盒子，里面可以放行内或者块级元素

####  8.3.2. <a name='-1'></a>行内元素
常见的行内元素：

    <a> <strong> <b> <em> <i> <del> <s> <ins> <span>，其中span是典型的行内元素

行内元素的特点：
1. 相邻的行内元素在一行上，可以显示多个
2. 无法设置宽、高
3. 默认宽度就是本身内容的宽度
4. 行内元素只能容纳文本和其他行内元素
注意点：
1. a元素内部能在放a
2. a元素中可以放块级元素

####  8.3.3. <a name='-1'></a>行内块元素
行内元素有几个特殊的标签 img input td，他们同时具有块元素和行内元素的特点，有些资料称他们为行内块元素。
行内块元素的特点：
1. 和相邻的行内元素（行内块）在一行上，但是他们之间会有空白缝隙。一行可以显示多个（行内元素特点）
2. 默认宽度是它本身内容的宽度（行内元素特点）
3. 高度、行高、外边距以及内边距都可以控制（块级元素的特点）

####  8.3.4. <a name='-1'></a>元素显示模式转换
特殊情况下我们需要元素模式转换：一个模式需要另一个模式的特性。比如想要增加a的点击范围
1. 转换为块级元素 display: block;
2. 转换成行内元素 display: inline;
3. 转换为行内块元素 display: inline-block;

##  9. <a name='-1'></a>盒子模型
页面布局的三大核心：盒子模型、浮动、定位

###  9.1. <a name='-1'></a>盒子模型的组成
+ border边框
+ content内容
+ padding内边距
+ margin外边距

####  9.1.1. <a name='border'></a>border
盒子边框大小，设置盒子边框会影响盒子实际大小
border: border-width || border-style || border-color
border-style: solid（实线）| dashed（虚线）| dotted（点线）
border-top 上边框
border-bottom 下边框
border-collapse: collapse 合并相邻的边框

##### 圆角边框
border-radius: length;
radis半径原理：圆与边框交集形成圆角效果

圆形的做法：
border-radius设置为宽度和高度的一半：

    width: 200px;
    height: 200px;
    border-radius: 50%;

圆角矩形的做法：
border-radius设置高度的一半:

    width: 300px;
    height: 100px;
    border-radius: 50px;
####  9.1.2. <a name='box-shadow'></a>box-shadow盒子阴影
box-shadow: h-shadow v-shadow blur spread color inset;

    box-shadow: 10px 10px 10px -4px rgba(0,0,0,.3);

| 值      | 描述                                         |
| ------- | -------------------------------------------- |
| blur    | 可选。模糊距离。                             |
| color   | 可选。阴影的颜色。                           |
| h-shdow | 必需。水平阴影的位置。允许负值。             |
| inset   | 可以选择。将外部阴影（outset）改为内部阴影。 |
| spread  | 可选。阴影的尺寸                             |
| v-shdow | 必需。垂直阴影的位置。允许负值               |

####  9.1.3. <a name='padding'></a>padding
padding: 内边距，设置内边距会影响盒子实际大小
如果盒子本身没有设置高度宽度，padding不影响盒子大小
padding-left
padding-right
padding-top
padding-bottom

padding复合写法：属性可以有一到四个值
+ padding: 5px;  代表上下左右
+ padding: 5px 10px; 代表上下5，左右10
+ padding: 5px 10px 20px; 代表上5 左右10 下20
+ pading: 5px 10px 20px 30px; 上 右 下 左o

####  9.1.4. <a name='margin'></a>margin
margin属性用于设置外边距，即控制盒子和盒子之间的距离
margin-left
margin-right
margin-top
margin-bottom

margin复合写法与padding一致

##### 外边距典型应用
外边距盒子可以让块级盒子**水平居中**，但是必须满足两个条件：
+ 盒子必须指定了宽度（width）
+ 盒子左右的外边距都设置为auto

    margin: 0 auto;

##### 嵌套块元素垂直外边距的塌陷
对于两个嵌套关系（父子关系）的块元素，父元素有上外边距同时子元素也有上外边距，此时父元素会探险比较大的外边距值
解决方案：
+ 可为父元素定义上边框  border: 1px solid transparent
+ 可为父元素定义上内边距 padding: 1px
+ 可以为父元素添加overflow:hidden
还有其他方法，比如浮动、固定，绝对定位的盒子不会有塌陷问题

##### 清除内外边距
网页元素很多都带有默认的内外边距，而且不同浏览器默认的也不一致。因此我们在布局前，首先要清除下网也元素的内外边距

    * {
        padding: 0;
        margin: 0;
    }

##  10. <a name='-1'></a>传统网页布局的三种方式
###  10.1. <a name='-1'></a>普通流(标准流)
所谓标准流就是按照标签规定好的默认方式排列
+ 块级元素独占一行
+ 行内元素从左到右，遇到父元素的边缘则自动换行
###  10.2. <a name='-1'></a>浮动
float: none | left | right
float用于创建浮动框，将其移动到一边，直到左边缘或右边缘触及包含块或另一个浮动框的边缘
| 属性值 | 描述             |
| ------ | ---------------- |
| left   | 元素向左浮动     |
| none   | 元素不浮动默认值 |
| right  | 元素向右浮动     |
####  10.2.1. <a name='-1'></a>浮动的特性
1. 浮动元素会脱离标准流（脱标）,浮动的盒子只会影响后面的标准流
2. 浮动的元素会一行内显示并且元素顶部对齐
3. 浮动的元素会具有行内块元素的特性，任何元素都可以添加浮动
####  10.2.2. <a name='-1'></a>浮动元素和标准流父级搭配使用
先用标准流的父元素排列上下位置，之后内部子元素采取浮动排列左右位置。
####  10.2.3. <a name='-1'></a>清除浮动
clear: left | right | both;
| 属性值 | 描述                                       |
| ------ | ------------------------------------------ |
| both   | 同时清除左右两侧的影响                     |
| left   | 不允许左侧有浮动元素（清除左侧浮动的影响） |
| right  | 不允许右侧有浮动元素（清除右侧浮动的影响） |
由于父级盒子很多情况下，不方便给高度，但是盒子浮动又不占有位置，最后父级盒子高度为0时，就会影响标准流盒子
+ 清除浮动的本质是清除浮动元素造成的影响
+ 如果父盒子本身有高度，则不需要清除浮动
+ 清除浮动之后，父级就会根据浮动的子盒子自动检测高度。父级别有了高度，就不会影响下面的标准流了。
##### 清除浮动的方法
###### 额外标签法
也称为隔墙法，是W3C推荐的做法

额外标签法会在浮动元素末尾添加一个空格标签或者其他标签如br等块级元素

    <div style="clear:both"></div>

+ 优点：通俗易懂，书写方便
+ 缺点：添加许多无意义的标签，结构比较差
###### 父级添加overflow属性
可以给父级添加overflow属性，将其属性值设置为hidden、auto和scroll

    overflow: hidden

+ 优点：代码简介
+ 缺点：无法显示溢出的部分
###### 父级添加after属性
:after伪元素方式是额外标签法的升级版。也是给父元素添加

    .clearfix:after {
        content: "";
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
    }

    .clearfix{ /*IE6、7专有*/
        *zomm: 1;
    }

+ 优点：没有增加标签，结构更简单
+ 缺点：照顾低版本浏览器
+ 代表网站：百度、淘宝、网易
###### 父级添加双伪元素
也是给腹肌元素添加

    .clearfix:before,.clearfix:after {
        content: "",
        display:table;
    }

    .clearfix:after {
        clear:both;
    }

    .clearfix {
        *zoom:1;
    }

+ 优点：代码更简洁
+ 缺点：照顾低版本浏览器
+ 代表网站：小米、腾讯等
###  10.3. <a name='-1'></a>定位
浮动可以让多个块级盒子一行没有缝隙排列显示，经常用于横向排列盒子。
定位则是可以让盒子自由的在某个盒子内移动位置或者固定屏幕中的某个位置，并且可以压住其他盒子。
定位 = 定位模式 + 边偏移
####  10.3.1. <a name='-1'></a>定位模式
**定位模式**用于指定一个元素在问低昂中的定位方式。边偏移则决定了该元素的最终位置。

定位模式通过css的**position**属性来这是，其值可以分为四个：
+ static 静态定位
+ relative 相对定位
+ absolute 绝对定位
+ fixed 固定定位
##### 静态定位（了解）
静态定位是元素默认的定位方式，无定位的意思。静态定位按照标准流特性摆放位置，它没有边偏移。

    选择器 {position: static}

##### 相对定位
**相对定位**是元素在移动位置时，根据原来位置来说的。

    选择器 {position: relative}

相对定位的特点：
+ 它是性对于自己原来的位置来移动的（移动位置的时候参照点事自己原来的位置）。
+ 原来在标准流的位置继续占有，后面的盒子仍然以彼岸之魂流的方式对待它（不脱标，继续保留原来的位置）。
####  10.3.2. <a name='-1'></a>边偏移
**边偏移**就是定位的盒子移动到最终位置。有top、bottom、left和right4个属性。
##  11. <a name='css-1'></a>css书写顺序
1. 布局定位属性：display/position/float/clear/visibility/overflow
2. 自身属性：width/height/margin/padding/border/background
3. 文本属性：color/font/text-decoration/text-align/vertical-align/white-space/break-word
4. 其他属性（css3）：content/cursor/border-radius/box-shadow/text-shadow/background:linear-gradient`