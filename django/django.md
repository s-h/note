### 查看版本
    >>> import django
    >>> print(django.get_version())
    2.0

    $ python3 -m django --version

### MVC设计模式
    模型(Model)、视图(View)、控制器(Controller)

### MTV设计模式
Django 对传统MVC设计模式进行了修改，将师徒分为View和Template，将动态逻辑处理与静态的页面展现分离开。而Model采用了ORM技术，将关系型数据库抽象成面向对象Python类，将表操作转换成类操作，为了避免复杂的SQL语句编写。
+ **模型(Model):** 用户封装与应用程序的业务逻辑相关数据及数据的处理方法，是web应用程序中用于处理应用程序的数据逻辑的部分，Model只提供功能性的接口，通过这些接口可以获取Model的所有功能。这个模块是Web框架和数据库的交互层。
+ **视图(View):** 负责显示和呈现。
+ **控制器(Controller):** 负责从用户端手机用户的输入，可以看成提供View的反向功能。

### 创建一个项目
    $ django-admin startproject mysite

### 启动django
    $ python3 manager.py runserver
    #访问127.0.0.1:8000

### 创建应用
app与project的 **区别**
+ 一个app实现某个功能
+ 一个project是配置文件和多个app的集合，这些app组成整个站点
+ 一个project可以包含多个app
+ 一个app可以属于多个project

app的存放文职可以是任何地点，通常放在manage.py脚本的同级目录下，创建命令:
    $ python3 manage.py startpp polls

### 创建模型(model)
**model** 的本质就是数据库表的布局，再附加一些元数据。每个模型就是python中的类
我们将创建两个模型：Question和Choice。Question包含一个问题和一个发布日期。Choice包含两个字段：该选项的文本描述和该选项的投票数。每一条Choice都关联到一个Question。这些都是由Python的类来体现，编写的全是Python的代码，不接触任何SQL语句。现在，编辑polls/models.py文件，具体代码如下：
	# polls/models.py

	from django.db import models

	class Question(models.Model):
		question_text = models.CharField(max_length=200)
		pub_date = models.DateTimeField('date published')

	class Choice(models.Model):
		question = models.ForeignKey(Question, on_delete=models.CASCADE)
		choice_text = models.CharField(max_length=200)
		votes = models.IntegerField(default=0)
每一个类都是django.db.models.Model的子类。每一个字段都是Field类的一个实例，每一个Field实例的名字就是字段的名字。

### 启用模型
上面的代码，包含两件事：
+ 创建该app对应的数据库表结构
+ 为Question和Choice对象创建基于Python的数据库访问API
启用模型操作：
	$ python3 manage.py makemigrations polls
	$ python3 manage.py sqlmigrate polls 0001
    $ python3 manage.py migrate                  #同步到数据库

### 创建管理员账号

    $ python manager.py createsuperuser

### 视图
一个视图就是一个页面
