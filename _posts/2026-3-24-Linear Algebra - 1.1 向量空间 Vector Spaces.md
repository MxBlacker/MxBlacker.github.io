---
date: 2026-01-04
layout: post
tag: Linear
title: LA - 1.1 向量空间
---
你会发现这节的标题和上节很像，但又不完全一样。诚然，**向量空间 Vector Space** 实际上是同济教材最后一个单元讲的，而且甚至是选修的部分。但是这里把它提到前面来讲，不仅是因为他能给我们更好的几何上的解释，而且它相较于枯燥的计算也更加有意思。

上一节我们讲了一个**数域**里面可以有许多关于**加法和数乘**美好的性质
- 运算交换律、运算结合律、数乘结合律 ($abx=a(bx)$)
- 每个加法都有一个逆元
- 加法和乘法之间由**分配律**联系着...

## 向量空间 Vector Spaces
>[!definition] $addition$
>$$\forall u,v\in V,u+v\in V$$

>[!definition] $Scaler\ Multiplication$
>见 [[Linear Algebra - 1 向量与空间#向量 vector|Scaler Multiplication]]
>$$\forall \lambda\in F,\forall v\in V,\lambda v\in V$$

那么我们不妨定义一个**向量空间**满足上面的这些性质

>[!definition] $Vector\ Space$
>向量空间由一个向量集 $V$，标量域 $F$，加法，数乘组成，满足以下性质：
>- Axiom1: 交换律$$\forall \vec{u},\vec{v} \in V,\vec{u}+\vec{v}=\vec{v}+\vec{u}$$
>- Axiom2,3: 结合律$$\forall \vec{u},\vec{v},\vec{w}\in V,\forall a,b\in F,(\vec{u}+\vec{v})+\vec{w}=\vec{u}+(\vec{v}+\vec{w})\ and\ (ab)\vec{v}=a(b\vec{v})$$
>- Axiom4: 加法单位元$$\exists 0\in V,\forall \vec{v}\in V,\vec{v}+0=\vec{v}$$
>- Axiom5: 加法逆元$$\forall \vec{v}\in V,\exists \vec{w}\in V,\vec{v}+\vec{w}=0$$
>- Axiom6: 乘法单位元$$\forall \vec{v}\in V,1\vec{v}=\vec{v}$$
>- Axiom7,8: 左乘/右乘分配律，这里不写了

有了上面的八条公理后，对于任意一个满足上面公理的集合，都可以被称为一个**向量空间**了！其实，基本上只要定义了加法和数乘就行。

你可以把向量空间里的每一个**元素**想象为一个箭头或者点，但它们可能实际上并不代表这些东西！
硬要说，向量空间本身也就是个集，只不过是加上了一些运算规则。

>[!attention] 向量空间不是一个域
>向量空间是一个**基于另一个给定的域** (我们这里是 $F$) 的**空间**，回忆一下我们给到的 [[Linear Algebra - 1 向量与空间#域 Field|域的定义]] ，发现向量空间其实是**没有定义向量间的乘法**的，所以不能算作一个域。
>准确来说，一个向量空间应该表示为 $$(V,F,+,\cdot)$$

#### 元组表示法 tuple notation
这里补充介绍一下一般数学结构的表示方法——**元组表示法**，具体怎么叫我不太清楚。
其形式为：
$$(承载合集,运算1,运算2,\cdots,特殊元素)$$
其中承载集合 (underlying set) 的定义为：

>An **underlying set** refers to the foundational set of elements that form the basis of mathematical structures, such as a **group**, **vector space**, or **topological space**.

- 向量空间就可以表示为 $(V,F,+,\cdot)$ 或者 $(V,+_V,\cdot_F)$
- 在之前介绍 [[Lambda - 3 自然数定义#皮亚诺公理 Peano Axioms|Peano Axioms]] 的时候也有提到一个自然数的定义 $(A,0,(.)*)$
- 群的定义为 $(G,\cdot,e,^{-1})$ ，即集合，二元运算符，单位元，逆运算符
***
所以我们一般不直接说 $V$，而是说 基于 $F$ 的 $V$ ( vector space over $F$ )
- 例如 $R^n$ 就是一个基于 $R$ 的向量空间，虽然 $R^n$ 也有乘法规则吧，但是那只是锦上添花的事，它确实满足了一个向量空间的条件。$C^n$ 也是如此。
- 最简单的一个向量空间为 $\{0\}$，即 $V=\{0\}$，这样无论你怎么搞都是 $\{0\}$

## 函数空间 function space
函数空间其实有点涉及到**泛函分析**一块了，但是这里还是提一下。
我们说了，向量其实可以是很多东西，当然这也包括函数。正常的实数空间 $\mathbb{R}^n$ 其实就是选定 $n$ 个正交的基底所张成的空间，那么一个函数空间就是用**一系列基函数张成**的空间，这么一个空间就由 $\mathbb{F}^\mathcal{S}$ 表示。就是我们接下来我们来定义一下 $\mathbb{F}^\mathcal{S}$ 的啥。

>[!definition] $\mathbb{F}^\mathcal{S}$ 函数空间
>如果 $\mathcal{S}$ 是一个集合，那么 $\mathbb{F}^\mathcal{S}$ 代表一个函数集合能够将 $\mathcal{S}$ 中的每一个值映射到 $\mathbb{F}$ 的某一个值上。即：
>$$\mathbb{F}^S = \{ f \mid f: S \to \mathbb{F} \}.$$

>[!example]
>假设 $\mathcal{S}$ 中的数为 $\{s_1,\cdots,s_n\}$ ，那么这个函数集合里的元素就可以是，比如说，$f$，它能做到 $f(s_1)=a_1, \cdots,f(s_n)=a_n$，其中 $a_1,\cdots,a_n\in F$；还可以是 $g$，使得 $g(s_1)=114514,\cdots,g(s_n)=1919810$ 。上面就是两个 $\mathcal{S}$ 到 $\mathbb{F}$ 的函数
>
>由于上面提到的函数 $f$ 仅由 $n$ 个值决定，其他值可以完全抛弃不管，所以你完全可以把一个函数列为**有序的实数对**，例如 $g\leftrightarrow (114514,\cdots,1919810)$
>所以每一个函数就可以可以由 $(f(1),\cdots,f(n))$ 表示了，其中 $n$ 就是 $\mathcal{S}$ 的长度

但是你要知道，大部分情况下我们处理的是**无限集**，也就是类似于 $\mathbb{R}^\mathbb{R}$，这就是我们日常生活中最常用的平面直角坐标系了，我们接触到的大部分函数都是属于这个集的。 $\mathbb{R}\to\mathbb{R}$，对于函数来说，就是输入一个实数，返回一个实数。当然，无限集和有限集的运算规则都还是一样的，唯一不同的不同就是项数。所以这里就不过多解释了。

我们再举一个例子，$\mathbb{R}^{[0,1]}$，这时候像 $f(x)=x，f(x) = \sin(2\pi x)$ 之类的都可以是里面的函数，**只不过 $x$ 的取值范围被限定要属于 $\mathcal{S}$**，也就是这里的 $[0,1]$。

简单来说，$S$ 相当于**定义域 domain**而 $F$ 相当于**值域空间 codomain**

>[!example] 接上面的提示
>这也就是说，对于一个有限集 $\mathcal{S}$ 来说，$R^\mathcal{S}$ 与 $R^n$ 同构。因为前后两者对于自己的 $n$ 个向量都可以随意取值。当然，这有更深刻的意义。
>可以将 $R^n$ 看作 $R^{\{1,\cdots,n\}}$ ，这样它里面的函数就表示为 $(f(1),\cdots,f(n))$ ，相当于 $R^n$ 中的每一个点。

>[!definition] $\mathbb{F}^\mathcal{S}$ 函数空间的性质
>对于加法有：$$\forall f,g\in\mathbb{F}^\mathcal{S},(f+g)(x)=f(x)+g(x)$$
>对于数乘有：$$\forall\lambda\in F,\forall f\in \mathbb{F}^\mathcal{S},(\lambda f)(x)=\lambda f(x)$$
>定义一个单位元：函数 $0$，即输入一个 $x$，总是返回一个 $0$，且**一个向量空间只有一个单位元** $$0(x)=0$$
>定义一个逆元：**一个向量空间每一个元素有且只有一个逆元** $$\forall f\in \mathbb{F}^\mathcal{S},(-f)(x)=-f(x)$$

这可以解释一个我们在学线代的时候的一个有趣现象，例如对任意矩阵 $A$ 有：
$$A\cdot0=O$$
这就说明一个向量**数乘完之后应当还是一个向量**








