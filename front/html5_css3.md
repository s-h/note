# web标准
## web标准的构成
### 结构
结构（Structure）用于对网页元素进行整理和分类：html
### 表现
表现（Presentation）用于表现设置网页元素的版式、颜色、大小等外观样式：css
### 行为
行为（Behavior）是指网页的定义及交互的编写：JavaScript
# html
# css
CSS是层叠样式（Cascading Style Sheets）的简称
## 选择器
选择器分为基础选择器和复合选择器
基础选择器包含：标签选择器、类选择器、id选择器和通配符选择器
### 标签选择器

    标签名 {
        属性 
    }

### 类选择器

    .类 {
        属性
    }

#### 类命名规则
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
### id选择器
使用#号定义

    #id{
        属性
    }

### 选择器优先级
!important > ID > 类(伪类、属性) > 标签
不推荐使用important优先级 
## 字体属性
+ font-size 字号
+ font-family
+ font-weight 字体粗细
+ font-style italic/normal
+ font 字体连写
## 文本属性
+ color 文本颜色 通常十六进制
+ text-align 文本对齐
+ text-indent 文本缩进
+ text-decoration 文本修饰 underline/none
+ line-height 行高
## css引入方式
+ 行内样式表
+ 内部样式表
+ 外部样式表
### 外部样式表
1. 新建.css外部样式表
2. <link rel"stylesheet" href="css文件路径">
## Emmet语法
它使用缩写，来提高html/css的书写速度
### 使用Emmet快速生成html
输入完成后需要使用tab键进行自动补全
1. 生成标签，直接输入标签
2. 生成相同的标签，标签*数字
3. 如果有父子级的标签，可以使用>，比如：ul>li
4. 如果有兄弟级别的标签，可以使用+，比如：div+p
5. 如果生成有类名或者id名字的，直接写.foo或者#bar，默认为div，可以使用：标签.id、标签#类名
6. 如果生成的div类名是有顺序的，可以使用自增符号$，例如.foo$*5生成class为foo1-foo5的div
7. 如果想要在生成标签中写内容可以使用{}，比如div{div中的内容}
### 使用Emmet快速生成css
css基本采取简写的形式使用tab进行补全
1. 比如w200 -> width: 200px;
2. 比如lh26 -> line-height: 26px;
3. 比如tac -> text-align: center;
