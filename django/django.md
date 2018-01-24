### 查看版本
    >>> import django
    >>> print(django.get_version())
    1.11

### MVC设计模式
    模型(Model)、视图(View)、控制器(Controller)

### MTV设计模式
Django 对传统MVC设计模式进行了修改，将师徒分为View和Template，将动态逻辑处理与静态的页面展现分离开。而Model采用了ORM技术，将关系型数据库抽象成面向对象Python类，将表操作转换成类操作，为了避免复杂的SQL语句编写。
+ **模型(Model):** 用户封装与应用程序的业务逻辑相关数据及数据的处理方法，是web应用程序中用于处理应用程序的数据逻辑的部分，Model只提供功能性的接口，通过这些接口可以获取Model的所有功能。这个模块是Web框架和数据库的交互层。
+ **视图(View):** 负责显示和呈现。
+ **控制器(Controller):** 负责从用户端手机用户的输入，可以看成提供View的反向功能。
