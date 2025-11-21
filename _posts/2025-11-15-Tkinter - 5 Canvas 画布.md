---
tags:
  - python
date: 2025-10-10
layout: post
title: Tkinter - Canvas 画布
---
Python Tkinter 画布（Canvas）组件和 **html5** 中的画布一样，都是用来绘图的。您可以将图形，文本，小部件或框架放置在画布上。

## 画布 Canvas 的定义

```Python
Canvas_w = tk.Canvas( master , option-value ...)
```

**其option有以下选项**：
- bd : 边框宽度
- bg : 背景色
- confine : 默认为True , 画布不可滚动到可滑动区域外
- cursor : 光标形状 ( arrow , circle , cross , plus )
- height: 画布高度
- highlightcolor : 高光色
- relief : 见[[Tkinter - 1 基本操作、类继承关系与部分组件#标签组件 Label|Label的Relief]]
- **scrollregion**：一个元组 tuple (w, n, e, s) ，定义了画布可滚动的最大区域，w 为左边，n 为头部，e 为右边，s 为底部
- **width**：画布在 X 坐标轴上的大小

## 类方法
```Python
Canvas_w.create_rectangle( x0 , y0 , x1 , y1 , start = 0 , extent = ... , fill = 'color' )
Canvas_w.create_arc( x0 , y0 , x1 , y1 , start = 0 , extent = ... , fill = 'color' )
Canvas_w.create_line(x0 , y0 , x1 , y1 , ... , xn , yn , options = '...')
Canvas_w.create_oval(x0, y0, x1, y1, options = '...')
Canvas_w.create_polygon(x0, y0, x1, y1,...xn, yn, options = '...')
Canvas_w.create_text(x0, y0, text = '...' , font = ('...' , x))

image = tk.PhotoImage(file = 'path')
Canvas_w.create_image(x0 , y0 , image = image)
```

这里的坐标似乎可以用元组表示
- **create_rectangle**：给定左上角和右下角的点
- **create_arc**：创建扇形。注意，**这里的圆弧是椭圆圆弧，坐标依旧是矩形坐标**，且圆弧是围着椭圆中心从十二点方向开始转的。**角度为弧度制**。（同pygame）
- **create_line**：画线，option可以是color和width...
- **create_oval**：画椭圆，$x_0 , y_0 , x_1 , y_1$ 代表一个矩形，椭圆被框定在其中
- **create_text**：直接画文本
- **create_image**：顾名思义

## 事件
点击画板发生的事情和触发的函数，这里event的实参是"\<Button-1\>"
```Python
 def on_click(event):
	 mouse_x = event.x
	 mouse_y = event.y 

 self.canvas.bind("<Button-1>", on_click)
```