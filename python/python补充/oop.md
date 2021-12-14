## 类和方法
### 类特殊方法和属性
#### __call___()
特殊的实例方法，该方法的功能类似于在类中重载 () 运算符，使得类实例对象可以像调用普通函数那样，以“对象名()”的形式使用。

    class CLanguage:
    # 定义__call__方法
    def __call__(self,name,add):
        print("调用__call__()方法",name,add)
    clangs = CLanguage()
    clangs("C语言中文网","http://c.biancheng.net")