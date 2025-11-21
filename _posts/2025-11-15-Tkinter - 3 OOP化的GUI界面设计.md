---
tags:
  - python
date: 2025-10-10
layout: post
title: Tkinter - OOP化的GUI界面设计
---
#tkinter
## Application <- Frame类
一般我们通过**Application**类来组织整个GUI，它继承了**Frame**类
**Frame**类是一个容器组件，可以容纳更多的组件，也就是说，此时**Application**类似于我们自定义的一个组件

在创建窗口后，我们创建的**Application**此时就相当于一个容器，能够容纳其他组建

#### 实例 #1
这里以官方HELLOWORLD为例

```python
import tkinter as tk

class Application(tk.Frame):#创建了一个Application类
    def __init__(self, master=None): #声明
        super().__init__(master)#继承了父类的信息，并传master的参数，这里的master是master_window
        self.master = master
        self.pack() #继承父类
        self.create_widgets()#创建基本布局，具体在下main定义了
		
    def create_widgets(self):
        self.hi_there = tk.Button(self)#这里的hi_there实例就是一个Button类型了
        self.hi_there["text"] = "Hello World\n(click me)"
        self.hi_there["command"] = self.say_hi #调用
        self.hi_there.pack(side="top")

        self.quit = tk.Button(self, text="QUIT", fg="red",
                              command=self.master.destroy)
        self.quit.pack(side="bottom")

    def say_hi(self):
        print("hi there, everyone!")

root = tk.Tk()#根窗口
app = Application(master=root)
app.mainloop()
```




