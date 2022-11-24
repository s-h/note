<!-- TOC -->

- [快速开始](#快速开始)
    - [查看版本](#查看版本)
    - [MVC设计模式](#mvc设计模式)
    - [MTV设计模式](#mtv设计模式)
    - [创建一个项目](#创建一个项目)
    - [启动django](#启动django)
    - [创建应用](#创建应用)
    - [创建模型(model)](#创建模型model)
    - [启用模型](#启用模型)
    - [创建管理员账号](#创建管理员账号)
- [详细讲解](#详细讲解)
    - [ORM](#orm)
        - [安装依赖](#安装依赖)
        - [配置mysql](#配置mysql)
    - [model](#model)
        - [默认值](#默认值)
        - [同步数据库](#同步数据库)
        - [新增数据](#新增数据)
    - [视图](#视图)
        - [请求](#请求)
        - [响应](#响应)
    - [模板](#模板)
        - [模板的语法](#模板的语法)
    - [静态文件](#静态文件)
        - [引用静态文件](#引用静态文件)

<!-- /TOC -->

## 快速开始
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


## 详细讲解
### ORM
ORM可以做帮助我们做两件事情
+ 创建、修改、删除数据库中的表（不用写SQL语句）【无法创建数据库】
+ 操作表中的数据（不用写SQL语句）
#### 安装依赖

    pip install mysqlclient

#### 配置mysql
setting.py配置数据库

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': '',
            'USER': '',
            'PASSWORD': '',
            'HOST': '',
            'PORT': ''
            }
    }

### model

    from django.db import models

    class UserInfo(models.Model):
        name = models.CharField(max_length=32)
        password = models.CharField(max_length=64)
        age = models.IntegerField()

    """
    create tables appname_userinfo(
        id bigint auto_increment primary key,
        name varchar(32),
        password varhcar(64),
        age int
    );
    """
#### 默认值
已有表新增字段，需要设置默认值或者允许为空

    #默认值
    age = models.IntergerField(default=10)

    #允许为空
    age = models.InterField(null=True, blank=True)

#### 同步数据库
生成sql    

    python manage.py makemigrations

    #只生成指定appsql
    python manage.py makemigrations app01


执行sql

    python manage.py migrate

#### 新增数据

    Model_name.objects.create(title="hello")

### 视图
一个视图就是一个页面

视图函数内部：
+ 读取含有模板语法的HTML文件
+ 内部进行渲染（模板语法执行并替换数据)
+ 将渲染完成的HTML返还给用户浏览器

#### 请求
request是一个对象，封装了用户发送过来的所有请求相关的数据

    def something(request):
        return Httpsponse("返回内容")

获取请求方式GET/POST

    request.method

获取GET/POST请求数据

    request.GET
    request.POST
    request.POST.get("user")

#### 响应
返回字符串

    return HttpResponse("字符串")

返回页面

    return render(request, "index.html", {"title": "foo"})

重定向

    return redirect("https://www.foo.com")

### 模板
模板放在templates里面
#### 模板的语法
在html中写一些占位符，由数据对这些占位符进行替换和处理

    views：
    name = "zhangsan"
    roles = ["admin", "user"]
    user_info = {"name": "foo", "role": "admin"}
    return render(request, "tpl.html", {"name": name, "roles": roles, "user_info": user_info})

    tmplates：
    -- 变量
    {{ name }}

    -- 列表
    {% for item in roles %}
       {{ item }}
    {% endfor %}

    -- 字典值
    {{ user_info.name }}
    {{ user_info.role }}

    -- 循环字典 keys
    {% for item in user_info.keys %}
        {{ item }}
    {% endfor %}

    -- 循环字典 value
    {% for item in user_info.values %}
        {{ item }}
    {% endfor %}

    -- 循环字典 key、values
    {% for k,v in user_info.items %}
        {{ k }} {{ v }}
    {% endfor %}

    -- 条件分支
    {% if name == "zhangsan"%}
        <h1>zhangsan</h1>
    {% else %}
        <h1>who are you<h1>
    {% endif %}


### 静态文件
静态文件放在statis里面
#### 引用静态文件
settings.py 中配置 STATIC_URL

    -- html开头
    {$ load statis %}
    -- 在需要引用静态文件的地方
    <img src="{% static 'js/jquery-3.6.0.min.js' %}" alt=""> 