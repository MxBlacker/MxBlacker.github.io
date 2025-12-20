---
layout: post
tag: Lambda
date: 2025-12-05
title: Lambda - 递归
---
我们以**阶乘**来举例，就叫$fac(n)$好了，首先在一个高级语言里面写好一个代码逻辑，这里用Python

```Python
def FACTORIAL(n):
	if n == 0:
		return 1
	else:
		return n * FACTORIAL(n - 1)
```

可见，有一个边界条件，即判断n是否为0，我们假设这个判断的函数为$IS\_0$，在将$n$应用于它后，它会返回一个布尔值，那么这个函数在lambda演算中就可以表示为
$$fac\equiv\lambda n.((IS\_0\ n)\ 1\ (×\ fac\ (-\ n\ 1)))$$

$IS\_0$实际上是[[2025-12-20-Lambda - 2 逻辑符号定义#零判断符 $isZero$|零判定符]]

#### 不动点
对于这种函数套函数的问题，我们中学其实做了很多了，一般有两种办法
- 再进行一次嵌套，从而替换
- **不动点**
显然，前者在这里是不管用的，所以我们用**不动点**来解决这里的循环定义问题

> 再次明确一下**不动点 fixed points**的定义
> 简单来说，对于任意一个函数图像$f(x)$，它与直线$y=x$的交点即为**不动点**
> 更准确的说，输入的值和输出的值是一样的

对于一般数学函数的不动点，有些是存在的，而有些在实数平面上是不存在的。
但是lambda演算中，函数的不动点**ALWAYS EXISTS**

##### 图灵不动点组合子 Turing Fixed Point Combinator
我们假设一个函数$U$为$\lambda xy.(y((xx)y))$，不管这个函数怎么来的，我们有这么个组合子叫做***图灵不动点组合子  Turing Fixed Point Combinator***
$$
\theta = UU
$$
对于任意一个函数$F$，我们将$\theta$应与于它，通过**β规约**可以得到
$$
\displaylines{
\theta{F}=F(\theta{F})\\
即F(\theta{F})=\theta{F}
}
$$
**也就是说，$\theta{F}$就是$F$的不动点！**
这个组合子可以用来表示任何lambda演算中函数的不动点！

***

回到我们需要解决的问题上来，我们不妨将$fac$换为$F$，内层的$fac$换位$f$，那么就有
$$
F=\lambda n.((IS\_0\ n)\ 1\ (×\ f\ (-\ n\ 1)))
$$
对两边应用不动点组合子，就有
$$
\theta F=(\lambda n.((IS\_0\ n)\ 1\ (×\ f\ (-\ n\ 1))))(\theta F)
$$
**β规约**
$$
\theta F=(\lambda n.((IS\_0\ n)\ 1\ (×\ (\theta F)\ (-\ n\ 1))))
$$
这时，你会惊奇的发现$\theta F$和$fac$的位置**一模一样**，也就是说$\theta F$**就是**$fac$函数！
注意！这里我们并没有循环定义！$F$的定义在上面被标明了，而$\theta$也只是一个组合子











