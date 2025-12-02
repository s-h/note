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
* 9. [元素的显示模式](#-1)
	* 9.1. [块元素](#-1)
	* 9.2. [行内元素](#-1)
	* 9.3. [行内块元素](#-1)
	* 9.4. [元素显示模式转换](#-1)
* 10. [盒子模型](#-1)
	* 10.1. [盒子模型的组成](#-1)
		* 10.1.1. [border](#border)
		* 10.1.2. [box-shadow盒子阴影](#box-shadow)
		* 10.1.3. [padding](#padding)
		* 10.1.4. [margin](#margin)
* 11. [传统网页布局的三种方式](#-1)
	* 11.1. [普通流(标准流)](#-1)
	* 11.2. [浮动](#-1)
		* 11.2.1. [浮动的特性](#-1)
		* 11.2.2. [浮动元素和标准流父级搭配使用](#-1)
		* 11.2.3. [清除浮动](#-1)
	* 11.3. [定位](#-1)
		* 11.3.1. [定位模式](#-1)
		* 11.3.2. [边偏移](#-1)
		* 11.3.3. [定位的扩展](#-1)
* 12. [css书写顺序](#css-1)
* 13. [网页布局总结](#-1)
	* 13.1. [标准流](#-1)
	* 13.2. [浮动](#-1)
	* 13.3. [定位](#-1)
* 14. [元素的显示与隐藏](#-1)
	* 14.1. [display](#display)
	* 14.2. [visibility](#visibility)
	* 14.3. [overflow](#overflow)
* 15. [flex布局](#flex)
	* 15.1. [flex属性](#flex-1)
		* 15.1.1. [justfy-content定义主轴对齐方式](#justfy-content)
		* 15.1.2. [align-items定义交叉轴的对齐方式（单行时整体对齐）](#align-items)
		* 15.1.3. [flex-direction定义主轴方向（改变主轴方向）](#flex-direction)
		* 15.1.4. [flex-wrap控制是否换行](#flex-wrap)
* 16. [Grid 网格布局](#Grid)
	* 16.1. [设置网格布局](#-1)
	* 16.2. [对齐方式](#-1)
	* 16.3. [网格轨道](#-1)
	* 16.4. [网格间距](#-1)
* 17. [CSS高级技巧导读](#CSS)
	* 17.1. [精灵技术](#-1)
	* 17.2. [字体图标](#-1)
		* 17.2.1. [字体图标下载](#-1)
		* 17.2.2. [字体图标追加](#-1)
	* 17.3. [CSS三角](#CSS-1)
	* 17.4. [CSS用户界面样式](#CSS-1)
		* 17.4.1. [鼠标样式cursor](#cursor)
		* 17.4.2. [轮廓线outline](#outline)
		* 17.4.3. [防止文本域拖拽](#-1)
	* 17.5. [vertical-align属性应用](#vertical-align)
		* 17.5.1. [图片地测空白缝隙解决方案](#-1)
	* 17.6. [单行内文字溢出省略号显示](#-1)
	* 17.7. [多行内文字溢出省略号显示](#-1)
	* 17.8. [常见布局技巧](#-1)
		* 17.8.1. [margin负值巧妙运用，边框重叠](#margin-1)
		* 17.8.2. [文字围绕浮动元素](#-1)
		* 17.8.3. [三角强化](#-1)
	* 17.9. [CSS初始化](#CSS-1)
* 18. [ HTML5](#HTML5)
	* 18.1. [HTML5新增语义化标签](#HTML5-1)
	* 18.2. [HTML5多媒体标签](#HTML5-1)
	* 18.3. [HTML5新增input表单](#HTML5input)
	* 18.4. [HTML5新增表单属性](#HTML5-1)
* 19. [CSS3](#CSS3)
	* 19.1. [CSS3新增选择器](#CSS3-1)
		* 19.1.1. [属性选择器](#-1)
		* 19.1.2. [结构伪类选择器——选择第n个元素](#n)
		* 19.1.3. [伪元素选择器](#-1)

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
注意事项:**按照lvha顺序书写**，a链接在浏览器中有默认样式，实际开发中需要给链接指定样式。
1. a:link 选择所有未被访问的链接
2. a:visited 选择所有已被访问的链接
3. a:hover 选择鼠标指针位于其上的链接
4. a:active 选择活动链接（鼠标按下未弹起的链接）
```css
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

    实际开发中的写法
    a {
        color: #333;
        text-decoration: none;
    }
    a:hover {
        color: #369;
        text-decoration: underline;
    }
```
##### focus伪类选择器
:focus伪类选择器用于获得焦点的表单元素
焦点就是光标，一般情况input类表单元素才能获取，因此这个选择器也主要针对表单元素来说

    /* 把光标所在的input元素选取出来 */
    input: focus {
        background-color: yellow;
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
```css
div {
    width: 200px;
    height: 40px;
    background-color: pink;
    line-height: 40px;
}
```
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
属性描述了元素的背景图片。**实际开发中常见logo或者装饰性的小图片或者超大的背景图片**，优点是非常灵活便于控制位置。

###  6.3. <a name='-1'></a>背景平铺
background-repeat: repeat | no-repeat | repeat-x | repeat-y

repeat-x 沿着x轴平铺
repeat-y 沿着y轴平铺

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

##  9. <a name='-1'></a>元素的显示模式

###  9.1. <a name='-1'></a>块元素
常见的块元素：<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>，其中div是典型的块级元素
块元素的特点：
1. 独占一行
2. 高度、宽度、外边距及内边距都可以控制
3. 宽度默认是容器 （父级宽度）的100%
4. 是一个容器及盒子，里面可以放行内或者块级元素

注意：
+ 文字类的元素内不能使用块级元素
+ p标签里不能放div
+ h1~h6不能放其他块元素



###  9.2. <a name='-1'></a>行内元素
常见的行内元素：

    <a> <strong> <b> <em> <i> <del> <s> <ins> <span>，其中span是典型的行内元素

行内元素的特点：
1. 相邻的行内元素在一行上，可以显示多个
2. 无法设置宽、高
3. 默认宽度就是本身内容的宽度
4. 行内元素只能容纳文本和其他行内元素
注意点：
1. a元素内部能在放a
2. a元素中可以放块级元素，但给a转换成块级元素最安全

###  9.3. <a name='-1'></a>行内块元素
行内元素有几个特殊的标签 img input td，他们同时具有块元素和行内元素的特点，有些资料称他们为行内块元素。
行内块元素的特点：
1. 和相邻的行内元素（行内块）在一行上，但是他们之间会有空白缝隙。一行可以显示多个（行内元素特点）
2. 默认宽度是它本身内容的宽度（行内元素特点）
3. 高度、行高、外边距以及内边距都可以控制（块级元素的特点）

###  9.4. <a name='-1'></a>元素显示模式转换
特殊情况下我们需要元素模式转换：一个模式需要另一个模式的特性。比如想要增加a的点击范围
1. 转换为块级元素 display: block;
2. 转换成行内元素 display: inline;
3. 转换为行内块元素 display: inline-block;

##  10. <a name='-1'></a>盒子模型
页面布局的三大核心：盒子模型、浮动、定位


###  10.1. <a name='-1'></a>盒子模型的组成
+ border边框
+ content内容
+ padding内边距
+ margin外边距

####  10.1.1. <a name='border'></a>border
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
####  10.1.2. <a name='box-shadow'></a>box-shadow盒子阴影
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

####  10.1.3. <a name='padding'></a>padding
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

####  10.1.4. <a name='margin'></a>margin
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

##  11. <a name='-1'></a>传统网页布局的三种方式
###  11.1. <a name='-1'></a>普通流(标准流)
所谓标准流就是按照标签规定好的默认方式排列
+ 块级元素独占一行
+ 行内元素从左到右，遇到父元素的边缘则自动换行
###  11.2. <a name='-1'></a>浮动
float: none | left | right
float用于创建浮动框，将其移动到一边，直到左边缘或右边缘触及包含块或另一个浮动框的边缘
| 属性值 | 描述             |
| ------ | ---------------- |
| left   | 元素向左浮动     |
| none   | 元素不浮动默认值 |
| right  | 元素向右浮动     |
####  11.2.1. <a name='-1'></a>浮动的特性
1. 浮动元素会脱离标准流（脱标）,浮动的盒子只会影响后面的标准流
2. 浮动的元素会一行内显示并且元素顶部对齐
3. 浮动的元素会具有行内块元素的特性，任何元素都可以添加浮动
####  11.2.2. <a name='-1'></a>浮动元素和标准流父级搭配使用
先用标准流的父元素排列上下位置，之后内部子元素采取浮动排列左右位置。
####  11.2.3. <a name='-1'></a>清除浮动
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
###  11.3. <a name='-1'></a>定位
浮动可以让多个块级盒子一行没有缝隙排列显示，经常用于横向排列盒子。
定位则是可以让盒子自由的在某个盒子内移动位置或者固定屏幕中的某个位置，并且可以压住其他盒子。
定位 = 定位模式 + 边偏移
| 定位模式          | 是否脱标         | 移动位置           | 是否常用   |
| ----------------- | ---------------- | ------------------ | ---------- |
| absolute 相对定位 | 是（不占有位置） | 带有定位的父级     | 常用       |
| fixed 固定定位    | 是（不占有位置） | 浏览器可视区       | 常用       |
| relative 相对定位 | 否（占有位置）   | 相对于自身位置移动 | 常用       |
| static 静态定位   | 否               | 不能使用边偏移     | 很少       |
| sticky 粘性定位   | 否（占有位置）   | 浏览器可视区       | 当前阶段少 |
####  11.3.1. <a name='-1'></a>定位模式
**定位模式**用于指定一个元素在问低昂中的定位方式。边偏移则决定了该元素的最终位置。

定位模式通过css的**position**属性来这是，其值可以分为四个：
+ static 静态定位
+ relative 相对定位
+ absolute 绝对定位
+ fixed 固定定位
####  11.3.2. <a name='-1'></a>边偏移
**边偏移**就是定位的盒子移动到最终位置。有top、bottom、left和right4个属性。

##### 静态定位（了解）
静态定位是元素默认的定位方式，无定位的意思。静态定位按照标准流特性摆放位置，它没有边偏移。

    选择器 {position: static}

##### 相对定位
**相对定位relative**是元素在移动位置时，根据原来位置来说的。

    选择器 {position: relative}

相对定位的特点：
+ 它是性对于自己原来的位置来移动的（移动位置的时候参照点事自己原来的位置）。
+ 原来在标准流的位置继续占有，后面的盒子仍然以彼岸之魂流的方式对待它（不脱标，继续保留原来的位置）。

##### 绝对定位
**绝对定位absolute**是元素在移动位置的时候，是相对于它祖先元素来说的 
```css
    选择器 { position: absoloute;}
```
绝对定位的特点：
+ 如果**没有父元素**或者**父元素没有定位**，以浏览器为定位准（Document文档）
+ 如果祖先元素有定位（相对、绝对、固定），则以最近一级有定位祖先元素为参考点移动位置
+ 绝对定位不再占用原来位置（脱离标准流）

##### 子绝父相
**子绝父相**口诀的含义：字级是绝对定位的话，父级要用相对定位
+ 子级绝对定位，不会占有位置，可以放到父盒子里面的任何一个位置，不会影响其他的兄弟盒子
+ 父盒子需要加定位限制子盒子在父盒子内显示

##### 固定定位
**固定定位fixed**是**固定于浏览器可视区域位置**。主要使用场景：可以在浏览器页面滚动时元素的位置不会改变。
```css
选择器 {position: fixed;}
```
固定定位的特点：
+ 以浏览器的可视窗口为参照点移动元素
+ 跟父元素没有任何关系

##### 固定定位小技巧
固定到版心右侧方法：
+ 让固定定位的盒子left:50%，走到浏览器可视区（也可以看做版心）的一半
+ 让固定定位的盒子margin-left:版心宽度的一半距离，多走版心宽度的一半。
```css
    .fixed {
        position: fixed;
        left: 50%
        margin-left: xx(版心的一半)px;
    }
```
##### 粘性定位(了解)
**粘性定位sticky**可以认为是相对定位和固定定位的混合。
```css
    选择器 {
        position: sticky; top: 10px;
    }
``` 

粘性定位的特点
+ 以浏览器的可视窗口为参照点移动元素（固定定位特点）
+ 粘性定位占有原先的位置（相对定位特点）
+ 必须添加top、left、right、bottom其中一个才有效 
##### 定位叠放次序z-index
在使用定位布局时，可能会出现盒子重叠的情况。此时，可以使用z-index来控制盒子的前后次序

####  11.3.3. <a name='-1'></a>定位的扩展
##### 绝对定位的盒子实现居中
```css
/* 加了绝对定位的盒子不能使用marigin: 0 auto 来水平居中 */
    foo {
        position: absolute;
        /* 1. left走父容器宽度的一半 */
        left: 50%;
        /* 2. margin-left负值往左走自己盒子宽度的一半 */
        margin-left: -容器的一半px;
    }
```
##### 定位的特殊性
绝对定位和固定定位也和浮动类似
+ 行内元素添加绝对或者固定定位，可以直接设置高度和宽度
+ 块级元素添加绝对定位或者固定定位，如果不给宽度或者高度，默认大小是内容的大小

##### 绝对定位（固定定位）会完全压住盒子
浮动元素不同，只会压住它下面标准流的盒子，但是不会压住下面标准流盒子里面的文字
但是绝对定位（固定定位）会压住下面标准流所有的内容
浮动之所以不会压住文字，因为浮动产生的目的最初是为了做文字环绕效果的。文字会围绕浮动元素。

##  12. <a name='css-1'></a>css书写顺序
1. 布局定位属性：display/position/float/clear/visibility/overflow
2. 自身属性：width/height/margin/padding/border/background
3. 文本属性：color/font/text-decoration/text-align/vertical-align/white-space/break-word
4. 其他属性（css3）：content/cursor/border-radius/box-shadow/text-shadow/background:linear-gradient`

##  13. <a name='-1'></a>网页布局总结
通过盒子模型，清楚知道大部分html标签是一个盒子
通过css浮动、定位可以让每个盒子排列成为网页。
一个完整的网页，是标准流、浮动、定位一起完成布局的，每个都有自己的专门用法
###  13.1. <a name='-1'></a>标准流
可以让盒子上下排列或者左右排列，垂直的快级盒子显示就是用标准流布局
###  13.2. <a name='-1'></a>浮动
可以让多个块级元素一行显示或者左右对齐盒子，多个块级盒子水平显示就用浮动布局
###  13.3. <a name='-1'></a>定位
定位最大的特点是有层叠的概念，就是可以让多个盒子前后叠压来显示。如果元素自由在某个盒子内移动就用定位布局。

##  14. <a name='-1'></a>元素的显示与隐藏
类似网站广告，当我们点击关闭就不见了，但是我们重新刷新页面，会重新出现
本质：让一个元素在页面中隐藏或者显示出来
###  14.1. <a name='display'></a>display
display属性用于设置一个元素应如何显示
display: none; 隐藏对象
display隐藏对象后，不再占有原来的位置
应用广泛，配合JS可以做很多网页特效

###  14.2. <a name='visibility'></a>visibility
visibility: hidden; 元素隐藏
visibility: visible; 元素可视
visibility隐藏元素后继续占有原来的位置
###  14.3. <a name='overflow'></a>overflow
overflow属性指定了如果内容溢出了一个元素的框（超过其指定高度及宽度）时，会发生什么。
| 属性值  | 描述                                   |
| ------- | -------------------------------------- |
| auto    | 超出自动显示滚动条，不超出不显示滚动条 |
| hidden  | 超出部分隐藏                           |
| scroll  | 不管超出内容否，总是显示滚动条         |
| visible | 不剪切也不添加滚动条                   |


##### 绝对定位（固定定位）会完全压住盒子
浮动元素不同，只会压住它下面标准流的盒子，但是不会压住下面标准流盒子里面的文字
但是绝对定位（固定定位）会压住下面标准流所有的内容
浮动之所以不会压住文字，因为浮动产生的目的最初是为了做文字环绕效果的。文字会围绕浮动元素。

##  15. <a name='flex'></a>flex布局
Flexbox是CSS弹性盒子布局模块（Flexible Box Layout Module）的缩写，可以实现元素的**对齐、分布和空间分配**

弹性盒子核心：
+ **父控子**

父盒子控制子盒子如何排列
父盒子称为容器，子盒子称为项目

+ **主轴和交叉轴（侧轴）

主轴默认水平居中，侧轴默认垂直方向，可以更改

###  15.1. <a name='flex-1'></a>flex属性
| 属性            | 作用                                                                 | 示例                                     |
| --------------- | -------------------------------------------------------------------- | ---------------------------------------- |
| align-content   | 定义多行是交叉轴上的对齐方式（仅当 flex-wrap:wrap 且内容换行时生效） | .container{align-content:space-between;} |
| align-items     | 定义交叉抽对齐方式（当夯实整体对齐）                                 | .container{align-items:center;}          |
| display         | 定义元素为 Flex 容器                                                 | .container {display:flex;}               |
| flex-direction  | 定义主轴方向（项目排列方向）                                         | .container{flex-direction:row;}          |
| flex-wrap       | 控制是否换行                                                         | .container{flex-wrap:wrap;}              |
| justify-content | 定义主轴上的对齐方式（整体分布）                                     | .container{justfy-content:center;}       |


行内元素设置了flex布局，都可以直接设置高度和宽度

####  15.1.1. <a name='justfy-content'></a>justfy-content定义主轴对齐方式
| 属性值        | 效果             | 示例                                   |
| ------------- | ---------------- | -------------------------------------- |
| center        | 居中对齐         | 子元素居中                             |
| flex-end      | 右对齐           | 子元素靠右排列                         |
| flex-start    | 左对齐（默认）   | 子元素靠左排列                         |
| space-around  | 项目两侧间隔相等 | 每个子元素周围分配相同的空间           |
| space-between | 两端对齐         | 首个子元素放置于起点末尾元素放置于终点 |
| space-evenly  | 项目间隔俊宇分布 | 每个子元素之间间隔相等                 |

####  15.1.2. <a name='align-items'></a>align-items定义交叉轴的对齐方式（单行时整体对齐）
| 属性值     | 作藐描述                                       |
| ---------- | ---------------------------------------------- |
| center     | 项目在交叉轴居中对齐                           |
| flex-end   | 项目在交叉轴终点对齐                           |
| flex-start | 项目在交叉轴起点对其（默认值）                 |
| stretch    | 项目拉伸填充整个容器高度（需子项目无固定高度） |
####  15.1.3. <a name='flex-direction'></a>flex-direction定义主轴方向（改变主轴方向）
| 属性值         | 描述                                   |
| -------------- | -------------------------------------- |
| column         | 子元素沿垂直主轴（从上到下排列）       |
| column-reverse | 子元素沿垂直主轴反向排列（从下到上）   |
| row            | 默认值。子元素沿水平轴（从左到右）排列 |
| row-revers     | 子元素沿水平轴反向排列（从右到左）     |

####  15.1.4. <a name='flex-wrap'></a>flex-wrap控制是否换行
| 属性值 | 排列效果 |
| ------ | -------- |
| nowrap | 不换行   |
| wrap   | 换行     |


##  16. <a name='Grid'></a>Grid 网格布局
CSS Grid Layout网格布局是一种二维布局模型，允许开发者通过定义行（rows）和列（columns）来精确的控制网页元素的位置和尺寸。还可以实现响应式设计。

弹性布局flex，一维布局，只能在单一方向（水平或垂直）上排列元素，合适线性排列的场景。

**网格布局grid**，可以同时控制行和列的排列，实现真正的二维布局。

###  16.1. <a name='-1'></a>设置网格布局
容器（父盒子）设置**display:grid;（块级）或者display:inline-grid(行内)

默认只创建了一个只有一列的网格

**绘制网格轨道**
+ grid-template-columns 定义网格中的列
+ grid-template-rows 定义网格中的行

属性值：
有几个属性值代表创建几列/行
```css
a {
    display: grid;
    grid-template-columns: 200px 200px 200px;
    grid-template-rows: 200px 200px 200px;
}
```
###  16.2. <a name='-1'></a>对齐方式
**justify-content**是控制**列轨道（column tracks）**在容器内水平分布。
**align-content**是控制**行轨道（Row Tracks）**在容器内水平分布

| 属性值        | 水平方向效果                             | 垂直方向效果                            |
| ------------- | ---------------------------------------- | --------------------------------------- |
| center        | 水平居中对齐                             | 垂直居中对齐                            |
| end           | 右对齐                                   | 底部对齐                                |
| space-around  | 两侧留出相等的空白，项目周围空间均匀分布 | 上下流出相等的空白,项目周围空间均匀分布 |
| space-between | 首尾项目贴边                             | 上下项目贴边                            |
| space-evenly  | 项目间、首尾与边界的空白相等             | 项目间、首尾与边界的空白相等            |
| start(默认值) | 左对齐                                   | 顶部对齐                                |

###  16.3. <a name='-1'></a>网格轨道
grid-template-columns / grid-template-rows 属性值

| 属性值           | 说明                                   | 示例                                                     | 应用场景                   |
| ---------------- | -------------------------------------- | -------------------------------------------------------- | -------------------------- |
| **fr 单位**      | 分配轨道剩余空间的比例（1fr 表示一份） | grid-templelate-columns:1fr 2fr;                         | 需要自适应比例分配的布局   |
| **repeat()函数** | 简化重复的列定义                       | grid-template-columns:repeat(3,1fr);等效关于 1fr 1fr 1fr | 多列等宽布局               |
| 百分比           | 按容器宽度百分比分配列宽               | grid-template-columns:30% 70%;                           | 响应式布局中按比例划分列   |
| 固定长度         | 使用 px、em 等固定单位定义列宽         | grid-template-columns:100px 200px;                       | 需要精确控制列宽的固定布局 |
| auto             | 列宽由内容自动撑开                     | grid-template-columns:auto 100px;                        | 内容宽度不确定时的灵活布局 |
| minmax()函数     | 定义列宽的最小值和最大值               | grid-template-columns:minmax(100px, 1fr);                | 响应式布局中限制列宽范围   |

repeat()还可以自动填充：适用于响应式布局中"列数随容器宽度变化"的场景
+ autofill 尽可能多的创建列
+ autofit 尽可能拉伸列填满容器（会合并空白，列宽不小于minmax的最小值）

```html
<!-- repeat自动填充 -->
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
            width: 80%;
            height: 300px;
            border: 1px solid red;
            margin: 50px auto;
        }

        .box .item {
            height: 120px;
            background-color: pink;
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="item">1</div>
        <div class="item">2</div>
        <div class="item">3</div>
        <div class="item">4</div>
        <div class="item">5</div>
        <div class="item">6</div>
        <div class="item">7</div>
        <div class="item">8</div>
        <div class="item">9</div>
    </div>
    </div>
</body>
```
###  16.4. <a name='-1'></a>网格间距
**gap**简写属性用于设置行与列之间的间隙（网格间距）
```css
    a {
        gap: 20px; 行和列之间保持20像素
        gap: 20px 30px; 行间距是20像素，列间距是30像素
        column-gap: 20px; 列间距
        row-gap: 20px; 行间距
    }
    
```
### 网格线
使用场景：
+ 实现元素跨越多个网格单元

分为**行线**和**列线**，线**编号**从**1**开始计算，**3**列就会有**4**根线

如何实现**元素跨越**多个网格单元？
```css
/* 实现语法1 */
1. 跨列 grid-column: 开始线编号/结束线编号
2. 跨行 grid-row: 开始线编号/结束线编号
.foo {
    grid-column: 1/3; /* 1/3 表示从第1列到第3列 */
    grid-row: 1/3; /* 1/3表示从第1行到第3行 */
}

/* 实现语法2 */
1. 跨列:grid-column: 开始线编号/ span 跨单元格数量
2. 跨行:grid-row: 开始线编号/ span 跨单元格数量
.foo {
    grid-column:1 / span 2; /* 从1线开始，跨2个单元格，等价于第一种写法*/
    grid-row: 1 / span 2;
}
```

注意：该属性要加到**项目**（子元素）身上

##  17. <a name='CSS'></a>CSS高级技巧导读
###  17.1. <a name='-1'></a>精灵技术
精灵图sprites的使用核心：
+ 精灵技术主要针对背景图片使用。就是把多个小背景图片整合到一张大图中。
+ 这个大的图片称为精灵图或者雪碧图
+ 主要借助背景位置来实现——background-position
+ 一般情况下精灵图都是负值（千万注意网页中的坐标：x轴右边走时正值，左边走时负值，y轴同理）
```css
    foo {
        width: 60px;
        height: 60px;
        /* 可以使用background属性简写图片url x轴 y轴 */
        background:url(images/sprites.png) no-repeat -200px -100px;
    }
```
###  17.2. <a name='-1'></a>字体图标
字体图标使用场景：主要用于显示网页中通用、常用的一些小图标。
精灵图是有诸多优点的，但是缺点很明显：
+ 图片文件还是比较大的
+ 图片本身放大和缩小会失真
+ 一旦图片制作完毕想要跟换非常复杂
此时，有一种技术的出现很好的解决了以上问题，就是字体图标iconfont
字体图标可以为前端工程师提供一种方便高效的图标使用方式，展示的是图标，本质属于字体

使用步骤：
####  17.2.1. <a name='-1'></a>字体图标下载
推荐下载网站
+ icomoon.io
+ www.iconfont.cn

##### 字体图标引入
以icomoon为例，打开网站选择好图标，下载文件，fonts放到字体文件夹，style.css下列代码已入页面css
```css
@font-face {
  font-family: 'icomoon';
  src:  url('fonts/icomoon.eot?12tq33');
  src:  url('fonts/icomoon.eot?12tq33#iefix') format('embedded-opentype'),
    url('fonts/icomoon.ttf?12tq33') format('truetype'),
    url('fonts/icomoon.woff?12tq33') format('woff'),
    url('fonts/icomoon.svg?12tq33#icomoon') format('svg');
  font-weight: normal;
  font-style: normal;
  font-display: block;
}
```
根据demo.html复制字体图标到html中, 设置字体图标以及fontsize、color
```css
        span {
            font-family: 'icomoon';
            font-size: 100px;
            color: red;
        }
```

####  17.2.2. <a name='-1'></a>字体图标追加
以icomoon为例，将selection.json上传至icomoon.io，可以继续选择需要的字体图标
###  17.3. <a name='CSS-1'></a>CSS三角
```css
    .triangle {
        /* 定位实现在矩形周边的三角形 */
        position: absolute;
        right: -19px;
        top: 100px;
        /* 三角形必须宽度和高度为0 */
        width: 0;
        height: 0;
        /* 所有边框10px但透明 */
        border: 10px solid transparent;
        border-left-color: pink;
    }
```
###  17.4. <a name='CSS-1'></a>CSS用户界面样式
所谓**界面样式**，就是更改一些用户操作样式，以便提高用户体验
####  17.4.1. <a name='cursor'></a>鼠标样式cursor
| 属性        | 描述      |
| ----------- | --------- |
| default     | 小白 默认 |
| move        | 移动      |
| not-allowed | 禁止      |
| pointer     | 小手      |
| text        | 文本      |
####  17.4.2. <a name='outline'></a>轮廓线outline
给表单、文本域添加outline:0;或者outline:none样式之后,就可以去掉默认的蓝色边框
####  17.4.3. <a name='-1'></a>防止文本域拖拽
```css
    textarea {resize: none;}
```
###  17.5. <a name='vertical-align'></a>vertical-align属性应用
CSS的**vertical-align**属性使用场景：经常用于设置图片或者表单（行内块元素）和文字垂直对齐
官方解释：用于设置一个元素的垂直对齐方式，但是它只针对行内元素或者行内块元素有效，块元素无法使用
| 值       | 属性                                   |
| -------- | -------------------------------------- |
| baseline | 默认。元素防止在父元素的基线上         |
| bottom   | 把元素的顶端与行中最低的元素的顶端对齐 |
| middle   | 把此元素放置在父元素的中部             |
| top      | 把元素的顶端与行中最高元素的顶端对齐   |

####  17.5.1. <a name='-1'></a>图片地测空白缝隙解决方案
img属于行内块元素，默认对齐方式为baseline，在添加其父元素div边框时底部会有空白缝隙，需要把img对齐方式改为不是baseline
```css
    img {
        vertical-align: bottom;
    }
```
###  17.6. <a name='-1'></a>单行内文字溢出省略号显示
单行文本溢出显示省略号——必须满足三个条件
```css
div {
    /* 1. 先强制一行内显示文本 */
    white-space: nowrap;
    /* 2. 超出部分隐藏 */
    overflow: hidden;
    /* 文字用省略号代替超出部分 */
    text-overflow: ellipsis;
}
```
###  17.7. <a name='-1'></a>多行内文字溢出省略号显示
```css
多行文本溢出显示省略号，有较大兼容性问题，合适于webkit浏览器或移动端（移动端大部分是webkit内核）
div {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
}
```
###  17.8. <a name='-1'></a>常见布局技巧
####  17.8.1. <a name='margin-1'></a>margin负值巧妙运用，边框重叠
两个有边框的盒子，两个挨着的边框会显得较粗，使用margin负值让边框重叠（实际测试这个效果并不好）
+ 让每个盒子margin往左侧移动1px 正好压住相邻盒子边框
+ 鼠标经过某个盒子的时候，提高当前盒子的层级即可（如果没有定位，则加相对定位（保留位置），如果有定位，则加z-index。
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        ul li {
            position: relative;
            float: left;
            list-style: none;
            width: 150px;
            height: 200px;
            border: 1px solid red;
            margin-left: -1px;
        }

        ul li:hover {
            z-index: 1;
            border: 1px solid blue;
        }
    </style>
</head>

<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>5</li>
        <li>5</li>
        <li>5</li>
    </ul>
</body>

</html>

```
####  17.8.2. <a name='-1'></a>文字围绕浮动元素
左边图片，右边文字布局，巧妙运用浮动元素不会压住文字的特性，左边盒子浮动，文字就会环绕图片
####  17.8.3. <a name='-1'></a>三角强化
直角三角形
```css
    .box {
        width: 0;
        height: 0;
        border-top: 100px solid transparent;
        border-right: 50px solid skyblue;
        border-bottom: 0 solid blue;
        border-left: 0 solid green;
    }
```
###  17.9. <a name='CSS-1'></a>CSS初始化
不同浏览器对有些标签的默认值是不同的，为了消除不同浏览器对html文本呈现的差异，照顾浏览器的兼容，我们需要对CSS初始化
以京东CSS初始化为例
```css
    /* 清除所有标签内外边距 */
    * {
        margin: 0;
        padding: 0
    }

    /* 斜体文字不倾斜  */
    em,
    i {
        font-style: normal
    }

    /* 去掉li的小圆点 */
    li {
        list-style: none
    }


    img {
        /* 浏览器兼容性，低版本浏览器图片外面包含链接会有边框的问题 */
        border: 0;
        /* 取消图片底侧有空白缝隙的问题 */
        vertical-align: middle
    }

    button {
        /* 当鼠标经过按钮变成小手 */
        cursor: pointer
    }

    a {
        color: #666;
        text-decoration: none
    }

    a:hover {
        color: #ff0f23
    }

    button,
    input {
        font-family: PingFang SC, Source Han Sans CN, Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif
    }

    body {
        /* 文字更加清晰，抗锯齿形 */
        -webkit-font-smoothing: antialiased;
        background-color: #fff;
        font: 12px/1.5 PingFang SC, Source Han Sans CN, Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;
        color: #666
    }

    .hide,
    .none {
        display: none
    }

    /* 清除浮动 */
    .clearfix:after {
        visibility: hidden;
        clear: both;
        display: block;
        content: ".";
        height: 0
    }

    .clearfix {
        *zoom: 1
    }
```
##  18. <a name='HTML5'></a> HTML5
###  18.1. <a name='HTML5-1'></a>HTML5新增语义化标签
**HTML5**的新增特性主要针对以前的不足，增加了一些**新的标签**、**新的表单**和**新的表单属性**等
新增语义化标签：
+ header 头部标签
+ nav 导航标签
+ ariticle 内容标签
+ section 区域标签
+ aside 侧边栏
+ footer 尾部标签

###  18.2. <a name='HTML5-1'></a>HTML5多媒体标签
+ 音频 audito
+ 视频 video

video常见属性
| 属性     | 值        | 描述                                                    |
| -------- | --------- | ------------------------------------------------------- |
| autoplay | autopaly  | 自动播放（谷歌浏览器需要添加 muted 来解决自动播放问题） |
| controls | controls  | 显示播放控件                                            |
| height   | 像素      | 播放器高度                                              |
| loop     | loop      | 循环播放                                                |
| muted    | muted     | 静音播放                                                |
| poster   | imgurl    | 加载等待画面图片                                        |
| preload  | auto/none | 是否预加载视频（如果有 autoplay 忽略该属性）            |
| src      | url       | 视频 url 地址                                           |
| width    | 像素      | 播放器宽度                                              |


audito常见属性
| 属性     | 值       | 描述         |
| -------- | -------- | ------------ |
| autoplay | autoplay | 自动播放     |
| controls | controls | 显示播放控件 |
| loop     | loop     | 循环播放     |
| src      | url      | 音频地址     |

###  18.3. <a name='HTML5input'></a>HTML5新增input表单
HTML新增input类型
| input type 属性值 | 说明                          |
| ----------------- | ----------------------------- |
| color             | 生成一个颜色表单              |
| date              | 限制用户输入必须为日期类型    |
| email             | 限制用户输入必须为 Email 类型 |
| month             | 限制用户输入必须为月份类型    |
| number            | 限制用户输入必须为数字类型    |
| search            | 搜索框                        |
| tel               | 手机号码                      |
| time              | 限制用户输入必须为时间类型    |
| url               | 限制用户输入必须为 URL 类型   |
| week              | 限制用户输入必须为星期类型    |

###  18.4. <a name='HTML5-1'></a>HTML5新增表单属性
| 属性         | 值        | 说明                                                                       |
| ------------ | --------- | -------------------------------------------------------------------------- |
| autocomplete | off/on    | 当用户在字段开始键入时，浏览器基于之前键入过的值，显示出该字段中填写的选项 |
| autofocus    | autofocus | 自动聚焦属性，页面加载完成后自动聚焦到指定表单                             |
| multiple     | multiple  | 可以多选文件提交                                                           |
| placeholder  | 提示文本  | 表单提示信息，存在默认值将不显示                                           |
| required     | required  | 表单拥有该属性表示其内容不能为空，必填                                     |

##  19. <a name='CSS3'></a>CSS3
###  19.1. <a name='CSS3-1'></a>CSS3新增选择器
####  19.1.1. <a name='-1'></a>属性选择器
属性选择器可以根据元素特定属性来选择选择元素。这样就可以不用借助于类或者id选择器
| 选择符        | 简介                                  |
| ------------- | ------------------------------------- |
| E[att]        | 选择具有 att 属性的 E 元素            |
| E[att*="val"] | 匹配 att 属性且值中含有 val 的 E 元素 |
| E[att^="val"] | 匹配 att 属性且值以 val 开头的 E 元素 |
| E[att="val"]  | 选择具有 att 属性值等于 val 的 E 元素 |
| E[att$="val"] | 匹配 att 属性且以 val 结尾的元素      |

```css
    input[type=text] {
        color: pink;
    }
```
####  19.1.2. <a name='n'></a>结构伪类选择器——选择第n个元素
结构伪类选择器主要根据文档结构来选择元素，常用于根据父级选择器里面的子元素
####  19.1.3. <a name='-1'></a>伪元素选择器
