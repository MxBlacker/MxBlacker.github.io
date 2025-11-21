---
tags:
  - python
date: 2025-10-10
layout: post
title: Tkinter - Python作业实战
---
## 2048游戏
这里先说一下2048的逻辑处理

>对于每一个格子，往**dir**方向移动时，如果该方向没有格子，**则一直前进**，**直到遇见格子或者碰壁**
>若情况为前者，这额外判断是否与当前格子相等，相等则合并
>注意，**这里不需要管合并过后的格子是否需要继续前进**，因为结果是必然的，一旦合并，其格子**必将在之后的循环**中被检测到，并继续移动

我们的格子实质上使用**设定好长宽和背景颜色的Label**做成的，wasd用的**keyboard**模块进行判定
最后，**grid_value**和**cells**分开来记录，前者是**对游戏的实际模拟**，后者是**记录格子的位置**，用于显示**grid_value**

#### 2048游戏格的绘制
我们直接用一个列表去存**每一个格子组件**，毕竟列表啥都能存
排列我们用**grid**，但请注意，这里的父级是**grid_frame**，我们专门创建了一块frame给到游戏格
```Python
        self.cells = []                         #格子，这里直接用Label
        for i in range(4):
            row = []
            for j in range(4):
                cell = tk.Label(
                    self.grid_frame,
                    text="",
                    font=("Arial", 20, "bold"),
                    width=4,
                    height=2,
                    relief="raised",
                    borderwidth=3
                )
                cell.grid(row=i, column=j, padx=5, pady=5)
                row.append(cell)
            self.cells.append(row)
```

#### 移动判定 / 结束判定
其实这一块不关Tkinter什么事，但我还是放上来参考一下

```Python    
def move(self, direction):
        moved = False                   #用于判断这次移动是否成功，到时候传到其他地方用于判断
        if direction == "up":
            for j in range(4):
                for i in range(1, 4):
                    if self.grid[i][j] != 0:    #按顺序判断每一个格子
                        row = i
                        while row > 0 and self.grid[row-1][j] == 0:         #空格子移动
                            self.grid[row-1][j] = self.grid[row][j]
                            self.grid[row][j] = 0
                            row -= 1
                            moved = True
                        if row > 0 and self.grid[row-1][j] == self.grid[row][j]:        #合并的情况
                            self.grid[row-1][j] *= 2
                            self.score += self.grid[row-1][j]
                            self.grid[row][j] = 0
                            moved = True
           ......
```

其他三个方向的情况略了。
- 可以看到循环的时候忽略了i = 0的情况，这是因为这些格子本身就在最上面，没有移动的必要
- 移动完之后，将移动标记设为**True**，好**置一个标志位**，用于**提示更新界面**
- 最后再来判断可不可以移动

```Python
    def is_game_over(self):
        #这一部分是游戏结束的判断：分两块 -> 一是无空格子，二是无可合并相邻数字
        for i in range(4):
            for j in range(4):
                if self.grid[i][j] == 0:
                    return False

        for i in range(4):
            for j in range(4):
                if j < 3 and self.grid[i][j] == self.grid[i][j+1]:
                    return False
                if i < 3 and self.grid[i][j] == self.grid[i+1][j]:
                    return False

        return True
```

#### 重新开始
重新开始的初始化是比较好理解的，重点要学习的是下面**Game Over**框的去除

```Python
    def restart_game(self):
        self.grid = [[0 for _ in range(4)] for _ in range(4)]
        self.score = 0
        self.add_new_tile()
        self.update_grid()

        for widget in self.window.winfo_children():
            if isinstance(widget, tk.Label) and widget.cget("text") == "Game Over!":
                widget.destroy()
```

- **winfo_children**：这个方法可以知道当前父窗口下的子组件有哪些，输出一个列表
- **cget**：**获取这个组件的信息**，这里我们获取了"text"，用于判断是否是GameOver
- **destroy**：同样的，组件也可以用于destroy

## 计算器 Calculator
这一个的主要问题在于我自己给我自己设计的挑战——像windows计算器一样显示流程
#### 基本功能实现
这里的显示框我们用一个**输入框 Entry**来代替，**result_var**就是它绑定的变量。
- **cur_input**代表这一轮数字输入的值
- **previous_input**的作用在于计算，等下我们深入解析一下Calculate函数就知道什么用了
- **Operator**用于记录运算符

```Python
        self.current_input = ""
        self.result_var = tk.StringVar()
        self.result_var.set("0")
        self.operator = ""
        self.previous_input = ""
        self.new_input = True
```

输入数字有四种情况：
- 当前不在一个运算中，输入的数字的算式的开头 (此时cur_input = '0')
- 点完运算符，已经显示一个结果了，接着输入新的数字 (此时cur_input = '上一轮计算结果')
- 还在当前轮的数字输入

首先，我们要判断使用者点了哪个按钮，**按钮排版我们不再赘述**
```Python
	if value.isdigit() or value == '.':
            self.input_number(value)
            self.flow_var.set(self.operate_flow + self.current_input)

        elif value in ['+', '-', '×', '÷']:
            self.operate_flow += self.current_input
            self.operate_flow += value
            self.input_operator(value)
            self.flow_var.set(self.operate_flow)

        elif value in ['√','x²']:
            self.instant_operator(value)

        elif value == '=':
            self.calculate()
            self.operate_flow = ''
            self.flow_var.set(self.current_input)
            return

        elif value == 'C':
            self.clear()

        elif value == '←':
            self.backspace()
            self.flow_var.set(self.operate_flow + self.current_input)

        elif value == '±':
            self.negate()

        elif value == 'e':
            self.e()
            self.flow_var.set(self.operate_flow + self.current_input)

        elif value == 'π':
            self.pi()
            self.flow_var.set(self.operate_flow + self.current_input)
```

第一个if判断是否是数字，是的话就会去判断现在是什么状况
这里的**new_input**为布尔变量，用于判断这时候该不该清除current_input里的数字

显然，在输入了一个数字后，无论前面是什么情况，之后都应该是**情况三**，所以**new_input**理所当然为False，直到触发前两个情况的条件

```Python
    def input_number(self, num):
        if self.new_input:
            self.current_input = num
            self.new_input = False

        else:
            # 防止输入多个小数点
            if num == '.' and '.' in self.current_input:
                return

            self.current_input += num
        self.result_var.set(self.current_input)
```

运算符成立的条件是**cur_input**里有实际合法的值，者可以用**new_input == False**来判断
我们注意到，计算器在你输入下一个运算符之前，都会显示你当前轮的输入。按下后才会显示上一轮和上上轮的运算结果，所以这里我们需要一个**prev_input**来记录上上轮的结果。

> [!attention] **current_input**在你点下运算符后已经成为上一轮输入，直到你

```Python
    def input_operator(self, op):                       #输入计算符
        if self.current_input:
            if self.operator and not self.new_input:
                self.calculate()

            self.previous_input = self.current_input
            self.operator = op
            self.new_input = True
```

下面给出运算函数
计算完成后，当前显示的应该是**result**，但是注意这里属于情况二，所以**new_input**要设为True

```Python
            prev = float(self.previous_input)
            curr = float(self.current_input)
            if self.operator == '+':
                result = prev + curr
            elif self.operator == '-':
                result = prev - curr
            elif self.operator == '×':
                result = prev * curr
            elif self.operator == '÷':
                if curr == 0:
                    messagebox.showerror("错误", "除数不能为零！")
                    self.clear()
                    return
                result = prev / curr

            if result == int(result): #处理精度，否则整数后面会跟一个.0
                result = int(result)

            self.result_var.set(str(result))
            self.current_input = str(result)
            self.operator = ""
            self.previous_input = ""
            self.new_input = True
```

#### 新增功能 即时运算符
这个还是很好理解的，**new_input**我故意设置为**False**的，正常应该是**True**
```Python
    def sqrt(self):
        self.current_input = str(f"{math.pi:.7f}")  #保留七位小数
        self.result_var.set(self.current_input)
        self.new_input = False
```

#### 计算流显示 Calculate flow
你会发现我上面写的识别符号的代码里有很多带有**flow**的变量，那就是专门用于显示计算全过程的。只需要识别你每一次的输入的合法性，然后加到一个专门的字符串上就行了。
这里引入**纯运算输入 Raw Input**，按键里我设置为**Pr**，点一下会变红

```Python
        if value == 'Pr':
            self.mode = 'Normal_cal' if self.mode == 'Pure' else 'Pure'

            for widget in self.button_frame.winfo_children():
                if isinstance(widget, tk.Button) and widget.cget("text") == "Pr":
                    if self.mode == 'Normal_cal':
                        widget.config(bg = '#F0F0F0')
                    else:
                        widget.config(bg = 'red')
                        
            self.clear()
            return

        if self.mode == 'Pure':
            if value == '=':
                result = eval(self.operate_flow)
                self.current_input = result
                self.result_var.set(result)
                self.operate_flow = ''
                self.flow_var.set(self.operate_flow)

            elif value == '←':
                self.operate_flow = self.operate_flow[:-1]

            elif value == '×':
                self.operate_flow += '*'
                
            elif value == '÷':
                self.operate_flow += '/'

            else:
                self.operate_flow += value
                self.flow_var.set(self.operate_flow)

            return
```

这一串就是专门用来纯输入的，最终输出的结果会是按照加减乘除的优先级运算的结果，因为使用了**eval**，指的注意的一点是颜色的改变，和上面2048识别GameOver的方法如出一辙。

## 图片4x4华容道
梦回Win7！这个代码量也出奇的少，只有165行，还包括了许多注释
这个游戏的原理是，在点击一个块后，如果这个块四周有空白块，他就会移动到这个块去。说是移动，实际上可以理解为**和空白块交换**。

#### 鼠标点击事件绑定
```Python
        self.canvas.bind("<Button-1>", self.on_click)
```

这里的鼠标判定相当麻烦，你只能判定鼠标点击了画布，并获取鼠标的具体位置，也就是说，具体点了哪一块是根据鼠标的位置去判定的！

```Python
    def on_click(self, event):  #鼠标事件
        col = event.x // (self.tile_size + self.margin)
        row = event.y // (self.tile_size + self.margin)

        #检查点击是否在有效范围内
        if 0 <= row < self.grid_size and 0 <= col < self.grid_size:
        
            #检查点击的方块是否与空白块相邻
            if self.is_adjacent((row, col), self.empty_pos):
            
                #交换方块
                self.swap_tiles((row, col), self.empty_pos)
                self.empty_pos = (row, col)
                self.draw_board()

                #检查是否完成
                if self.is_solved():
                    messagebox.showinfo("Congrats", "！！有点强！！")
```

#### 版面存储 / 获胜判断 / 打乱
这里我采用了一维数组进行存储
```Python
    def init_game(self): #初始化游戏板
        self.board = []
        for i in range(self.grid_size * self.grid_size - 1):
            self.board.append(i + 1)
        self.board.append(0)  #0表示空白块
        self.empty_pos = (self.grid_size-1, self.grid_size-1)
        self.draw_board()
```

要判断是否获胜很简单，只需要检查索引对不对的上就行了

```Python
    def is_solved(self): #检查拼图是否完成
        for i in range(self.grid_size * self.grid_size - 1):
            if self.board[i] != i + 1:
                return False
                
        return self.board[-1] == 0  #最后一块应该是空白
```

打乱的思路比较有趣，为了确保有解性，我们可以进行倒推

```Python
    def shuffle(self):
        '''
        这里有一个很有趣的思路，我们操作的对象实际上是空白格！因为要确保有解，所以只能靠一点点移动来打乱，这时候如果操作和空白格相邻的图片格会显得非常麻烦
        但是换个思考角度，操作实质上是格子的交换，我们也可以把目光聚焦在空白格上，这样就完美解决了问题
        '''

        moves = 100
        directions = [(0, -1), (0, 1), (-1, 0), (1, 0)] # 上下左右四个方向
        for i in range(moves):
            dx, dy = random.choice(directions)
            new_row, new_col = self.empty_pos[0] + dx, self.empty_pos[1] + dy
            if 0 <= new_row < self.grid_size and 0 <= new_col < self.grid_size:
                self.swap_tiles(self.empty_pos, (new_row, new_col))
                self.empty_pos = (new_row, new_col)
```

#### 图片分割
嗯嗯，这里用到了**Pillow**模块，所以以后有机会再讲讲
```Python
    def load_and_split_image(self):
        try:
            image_path = "miku.jpg" #我并没有考虑图片不存在的情况，要是出问题了，自己来这里改一下路径
            #加载并调整图片大小
            original_image = Image.open(image_path)
            image_size = self.grid_size * self.tile_size
            resized_image = original_image.resize((image_size, image_size), Image.Resampling.LANCZOS)

            #分割图片
            self.tile_images = []
            for row in range(self.grid_size):
                for col in range(self.grid_size):
                #确认需要裁剪的四个边界
                    left = col * self.tile_size
                    upper = row * self.tile_size
                    right = left + self.tile_size
                    lower = upper + self.tile_size
                    
                    tile_image = resized_image.crop((left, upper, right, lower))
                    self.tile_images.append(ImageTk.PhotoImage(tile_image))

        except Exception as e:
            print(f"加载图片失败: {e}")
            self.create_color_tiles()
```
