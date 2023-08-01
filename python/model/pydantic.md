# BaseModel
[基本模型(BaseModel)使用](https://blog.csdn.net/IT_LanTian/article/details/123229717)

pydantic主要是一个解析库，而不是验证库。验证是达到目的的一种手段：建立一个符合所提供的类型和约束的模型。
pydantic**保证输出模型的类型和约束，而不是输入数据**。

## 基本使用
User这是一个模型，它有两个字段id，一个是整数，是必需的，name一个是字符串，不是必需的（它有一个默认值)

    from pydantic import BaseModel
 
    class User(BaseModel):
        id: int
        name = 'yo yo'

类型name是从默认值(字符串)推断出来的，因此不需要类型注释（但是请注意当某些字段没有类型注释时有关字段顺序的警告）

user这是一个实例User。对象的初始化将执行所有解析和验证，如果没有ValidationError引发，说明生成的模型实例是有效的。

user实例有 id 和 name 2个属性

    user = User(id='123')
    print(user.id)   # 123
    print(user.name)  # yo yo

那么如何知道初始化的时候，需要哪些必填字段？可以通过 __fields_set__ 方法

    print(user.__fields_set__)  # {'id'}

.dict() 可以将user对象的属性，转成字典格式输出，dict(user) 也是等价的

.json()可以将user对象的属性，转成json格式输出

    print(user.dict())  # {'id': 123, 'name': 'yo yo'}
    print(dict(user))  # {'id': 123, 'name': 'yo yo'}
    print(user.json())  # {"id": 123, "name": "yo yo"}

## BaseModel 模型属性
+ dict()  返回模型字段和值的字典
+ json()  返回一个 JSON 字符串表示dict()
+ copy() 返回模型的副本（默认为浅拷贝）
+ parseobj() 如果对象不是字典，则用于将任何对象加载到具有错误处理的模型中的实用程序
+ parseraw() 用于加载多种格式字符串的实用程序
+ parsefile() 喜欢parseraw()但是对于文件路径
+ fromorm() 将数据从任意类加载到模型中
+ schema()  返回将模型表示为 JSON Schema 的字典
+ schemajson()  schema()返回;的 JSON 字符串表示形式
+ construct()  无需运行验证即可创建模型的类方法
+ __fields_set__初始化模型实例时设置的字段名称集__fields模型字段的字典
+ __config  模型的配置类

## 递归模型

    from typing import List
    from pydantic import BaseModel
    
    class Foo(BaseModel):
        count: int
        size: float = None
    
    class Bar(BaseModel):
        apple = 'x'
        banana = 'y'
    
    class Spam(BaseModel):
        foo: Foo
        bars: List[Bar]
    
    m = Spam(foo={'count': 4}, bars=[{'apple': 'x1'}, {'apple': 'x2'}])
    print(m)
    #> foo=Foo(count=4, size=None) bars=[Bar(apple='x1', banana='y'),
    #> Bar(apple='x2', banana='y')]
    print(m.dict())
    """
    {
        'foo': {'count': 4, 'size': None},
        'bars': [
            {'apple': 'x1', 'banana': 'y'},
            {'apple': 'x2', 'banana': 'y'},
        ],
    }
    """

## 辅助函数
### parse_obj
需要一个字典而不是关键字参数。

如果传递的对象不是 dict，ValidationError则将引发。

    from datetime import datetime
    from pydantic import BaseModel, ValidationError
    
    class User(BaseModel):
        id: int
        name = 'John Doe'
        signup_ts: datetime = None
    
    m = User.parse_obj({'id': 123, 'name': 'James'})
    print(m)  # id=123 signup_ts=None name='James'

### parse_raw
这需要一个str或bytes并将其解析为json，然后将结果传递给parse_obj. 通过适当地设置参数也支持解析泡菜数据。

    m = User.parse_raw('{"id": 123, "name": "James"}')
    print(m)  # > id=123 signup_ts=None name='James'

### parse_file
这需要一个文件路径，读取文件并将内容传递给parse_raw. 如果content_type省略，则从文件的扩展名推断。

    from pathlib import Path
 
    path = Path('data.json')  # 读取文件内容
    m = User.parse_file(path)
    print(m)  # > id=123 signup_ts=None name='James'

## 个人使用
如果数据层次很深，嵌套很多，可以把通用数据提取为dict，需要个性化的数据使用函数转换

    from pydantic import BaseModel

    class Bar(BaseModel):
        balabala: 'balabala'
        
    class Foo(BaseModel):
        balabala: 'balabala'

    class Foos(BaseModel):
        bar: Bar
        foo: List[Foo]

    def create_data(bar:Bar, foo:List[Foo]) -> dict:
        data = {
            "data": [{
                "ID": 1,
                "balabala": {
                    "balabala": "balabala"
                },
                'foos': {}
            }],
            'timestamp': 'timestamp'
        }

        foos = Foos(bar=Bar, foo=foo)
        data["data"]["foos"] = foos.dict()

    f1 = Foo(balabala='f1')
    f2 = Foo(balabala='f2')
    bar = Bar()
    data = create_data(bar=Bar, foos=[f1, f2])
