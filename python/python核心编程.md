## python核心编程 ##
### 第19章 图形用户界面编程 ###
#### 19.1 简介 ####
**Tkinter**是python的默认GUI库.
使用**Tkinter**

    import Tkinter

#### Tkinter 与 pyhon编程 ####
要创建并运行你的GUI程序,下面五步是基本的:  

    1.导入Tkinter模块
    import Tkinter 或者 from Tkinter import *
    2.创建一个顶层窗口对象，来容纳你的整个GUI程序
    top = TKinter.Tk()
    3.在你的顶层窗口对象上(或者说在“其中”)创建所有的GUI程序(以及功能)
    4.把这些GUI程序与底层代码相链接.
    5.进入主事循环
    Tkinter.mainloop()

例 19.1:  

    #!/usr/bin/env python
    #coding:utf-8
    import Tkinter
    top = Tkinter.Tk()
    label = Tkinter.Label(top,text = '你好啊!')
    label.pack()
    Tkinter.mainloop()

例 19.2 按钮组件:

    #!/usr/bin/env python
    import Tkinter
    top = Tkinter.Tk()
    quit = Tkinter.Button(top, text = 'hello world',
           command = top.quit)
    quit.pack()
    Tkinter.mainloop()

_按钮有一个额外的参数,Tkinter。quit()方法_  
例 19.3 标签和按钮:  

    #!/usr/bin/env python
    import Tkinter
    top = Tkinter.Tk()
    
    hello = Tkinter.Label(top, text='Hello World!')
    hello.pack()
    quit = Tkinter.Button(top, text='QUIT',
            command=top.quit, bg='red', fg = 'white')
    quit.pack(fill=Tkinter.X,expand=1)
    Tkinter.mainloop()

_fill参数告诉packer让QUIT按钮填充水平方向的剩余空间,而不必像以前那样大都使用缺省参数_  

例子 19.4 标签、按钮和进度条组件
