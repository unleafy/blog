---
title: '2025 Summer Day16'
description: '暑期集训 Day16'
date: '2025-07-30T03:22:39.534Z'
draft: false
tags:
  - 数学
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day16


**Content**：生成函数，多项式，期望

**Date**：2025.8.1



# 课堂内容

## 生成函数

### 定义

+ 普通生成函数（OGF）：普通生成函数的定义为形式幂级数：$\displaystyle F(x) = \sum_{i} a_i x^i$
+ 指数生成函数（EGF）：指数生成函数的定义为形式幂级数：$\displaystyle F(x) = \sum_{i} a_i \frac{x^i}{i!}$
      
### 普通生成函数的基本运算

+ $\displaystyle F(x) \pm G(x) = \sum_{i} (a_i \pm b_i) x^i$
+ $\displaystyle F(x)G(x) = \sum_{n} x^n \sum_{i=0}^n a_i b_{n-i}$
+ 封闭形式。Like: $\displaystyle F(x) = \sum_{n \ge 0} x^n = \frac{1}{1-x}$

证明：

$$
\begin{aligned}
& F(x) = 1 + x + x^2 + x^3 + \dots + x^n \\
& xF(x) = x + x^2 + x^3 + x^4 + \dots + x^(n+1) \\
& \therefore F(x) - xF(x) = 1 \to F(x) = \frac{1}{1-x}
\end{aligned}
$$

其他的普通生成函数也可以由这样的变换得到其封闭形式。

### 指数生成函数的基本运算

$$
\begin{aligned}
F(x)G(x) &= \sum_{i \ge 0} a_i \frac{x^i}{i!} \sum_{j \ge 0} b_j \frac{x^j}{j!} \\
&= \sum_{n \ge 0} x^n \sum_{i=0}^n \binom{n}{i} a_i b_{n-i} \frac{1}{i!(n-i)!} \\
&= \sum_{n \ge 0} \frac{x^n}{n!} \sum_{i=0}^n \binom{n}{i} a_i b_{n-i}
\end{aligned}
$$
  
即 $F(x)G(x)$ 的结果是序列 $\displaystyle \sum_{i=0}^n \binom{n}{i} a_i b_{n-i}$ 的指数生成函数。

# 例题

## [Luogu-P10780](https://www.luogu.com.cn/problem/P10780) BZOJ3028 食物

### 思路

我们把每个食物的生成函数学出来再相乘，得到：

$$
\begin{aligned}
& (\frac{1}{1-x^2})(x+1)(x^2+x+1)(\frac{x}{1-x^2})(\frac{1}{1-x^4})(x^3+x^2+x+1)(x+1)(\frac{1}{1-x^3}) \\
=& \frac{x}{x^4-4x^3+6x^2-4x+1} \\
=& \frac{x}{(x-1)^4}
\end{aligned}
$$

展开后得到：

$$
\sum_{n \ge 1} \frac{n(n+1)(n+2)x^n}{6}
$$

所以答案就为：$\displaystyle \frac{n(n+1)(n+2)}{6}$.最后，注意取模。

提交记录：[link](https://www.luogu.com.cn/record/228162980)

## [CodeForces-280C](www.vjudge.net/problem/CodeForces-280C) Game On Tree

### 思路

由于期望具有线性性：

$$
E(x+y) = E(x) + E(y)
$$

所以答案就是每个点被选中的概率之和。

因为每个点被选中的概率只和这个点到根节点的长度为 $dep(u)$ 的链有关（即 $u$ 的祖先）。所以这个点被选中的概率即为 $\displaystyle \frac{1}{dep(u)}$。答案即为：

$$
\sum_{u} \frac{1}{dep(u)}
$$

提交记录：[link](https://vjudge.net/solution/62432903)