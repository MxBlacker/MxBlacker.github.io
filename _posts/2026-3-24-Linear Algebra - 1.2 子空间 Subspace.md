---
date: 2026-01-04
layout: post
tag: Linear
title: LA - 1.2 笋子空间
---
这个很好理解，如果 $U$ 也是一个向量空间，且 $U$ 是 $V$ 的子集，那么 $U$ 就是 $V$ 的子空间。几何上来说，对于一个**三维的坐标系**，其中的一个**二维平面**就可以认为是一个子空间。需要注意一下的是 $V$ 本身也是 $V$ 的子空间。 

## 子空间的和 sum of subspaces
和涵数相加类似，我们假设有两个子空间分别为 $\mathcal{U}=\{(x_1,y_1,z_1,\cdots)\}$ 和 $\mathcal{W}=\{(x_2,y_2,z_2,\cdots)\}$，那么
$$\mathcal{U}+\mathcal{W}=\{{x_1+x_2,y_1+y_2,z_1+z_2,\cdots}\}$$
一个比较容易理解的推论是，**子空间的和是最小的包含指定子空间的子空间**，我们称其为**最小的包含子空间 smallest containing subspace**。这类似于集合论里的**给定两个某个集的子集，最小的包含这两个子集的子集是这两个子集的和**。这个结论可以记为：
$$span(\mathcal{V}_1\cup\cdots\cup\mathcal{V}_2)=\mathcal{V}_1+\cdots+\mathcal{V}_m$$
维度公式为：
$$\dim(V_1+V_2) = \dim V_1 + \dim V_2 - \dim(V_1 \cap V_2)$$
## 直和 $\oplus$
>[!definition] $direct\ sum\ \oplus$
>$\mathcal{W}_i$ 为向量空间 $V$ 的子空间，若 $\mathcal{W}_1+\cdots+\mathcal{W}_k=\mathcal{V}$ 且 $\mathcal{W}_1,\cdots,\mathcal{W}_k$ 相互独立，则称 $V$ 是子空间 $\mathcal{W}_1,\cdots,\mathcal{W}_k$ 的**直和 direct sum**，记为：
>$$\mathcal{V}=\mathcal{W}_1\oplus\cdots\oplus\mathcal{W}_k$$
> 你会发现这和上面大的直接相加似乎差不多，但是直和要求**每个向量的表示唯一**，更接近的意思是两个空间**正交**，虽然说独立不保正交。所以说，不相互独立的空间就不能和直和搭配使用。
> $$\mathcal{V}_i\cap\mathcal{V}_j=\{0\}$$
> 其维度公式为：
> $$dim(\mathcal{V}_i+\mathcal{V}_j)=dim\mathcal{V}_i+dim\mathcal{V}_j$$
> 

进一步解释一下上面的意思，每一个子空间内我们取一个向量 $v_k$ ，那么和子空间的向量就可以表示为 $\Sigma v_k$，这也是我们上面说的子空间相加的定义。**直和**要求 $\Sigma v_k$ 只有一种表示形式，此时每个空间所挑选的 $v_k$ 是固定的。
一个比较方便的验证方法是，对于一个直和出来的空间，其表示 $0$ 的方法当且仅当所有 $v_k$ 为 $0$。

>[!tips] 异或 $⊕$
>我们注意到 $⊕$ 其实也有异或的意思，这和**直和**其实有比较微妙的联系。
>我们在讲 [[Linear Algebra - 1 向量与空间#域 Field|数域]] 的时候提到了一个最小的域，即 $\{0,1\}$，其中 $1+1=0$，这描述的就是**异或运算**！其中乘法表示为**逻辑AND**。
>这就是**逻辑电路**这门课需要学习的，其具体定义如下：
>$$A \oplus B = (A \land \neg B) \lor (\neg A \land B)$$
>
>在抽象代数的视角下，两者都是模2整数域 $\mathbb{F}_2$ 上的向量空间运算的表示。
>更一般的，在**阿贝尔群**理论中，群的运算通常用 $\oplus$ 和 $+$ 表示，线性代数中的直和概念就是阿贝尔群直和中的特里。群论相关的我们以后有机会再学。






