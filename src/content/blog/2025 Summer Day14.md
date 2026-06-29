---
title: '2025 Summer Day14'
description: '暑期集训 Day14'
date: '2025-07-28T03:22:39.534Z'
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

# 2025 Summer Day14


**Content**：数论

**Date**：2025.7.30



# 课堂内容

## 莫比乌斯函数

定义如下：

$$
\mu(n) = \begin{cases}
1 & n=1 \\
(-1)^k & n = p_1 p_2 p_3 \dots p_k, \forall p_i \in P \\
0 & otherwise \\
\end{cases} 
$$

其中莫比乌斯函数有如下性质：

$$
\sum_{d|n} \mu(d) = [n=1]
$$

## 欧拉函数

定义如下：

$$
\varphi(n) = \sum_{i=1}^n [gcd(n,i)=1]
$$

其中欧拉函数具有如下性质：

$$
\sum_{d|n} \varphi(d) = n
$$

## 莫比乌斯反演

对于任意两个数论函数 $f(n)$，$g(n)$，有如下推导：

$$
f(n) = \sum_{d|n} g(d) \Leftrightarrow g(n) = \sum_{d|n} \mu(d) f(\frac{n}{d})
$$

# 例题

## [Luogu-P2257](https://www.luogu.com.cn/problem/P2257) YY的GCD

### 题意
给定 $n$，$m$，求：
$$
\sum_{i=1}^n \sum_{j=1}^m [gcd(i,j) \in Prime]
$$

### 思路

课上没怎么听懂，自己推了下竟然推出来了。

我们看到 $[gcd(i,j) \in Prime]$，考虑从质数入手，可以枚举质数，这样式子化为：

$$
\sum_{i=1}^n \sum_{j=1}^m \sum_{p \in Prime} [gcd(\frac{i}{p},\frac{j}{p})=1]
$$

把质数枚举提到前面去，得到：

$$
\begin{aligned}
& \sum_{p \in Prime} \sum_{i=1}^n \sum_{j=1}^m [gcd(\frac{i}{p}, \frac{j}{p}) = 1] \\ 
=& \sum_{p \in Prime} \sum_{i=1}^{\lfloor \frac{n}{p} \rfloor} \sum_{j=1}^{\lfloor \frac{m}{p} \rfloor} [gcd(i,j) = 1]
\end{aligned}
$$

对于后面一个埃德森括号，发现可以使用莫比乌斯函数的 $\displaystyle \sum_{d|n} \mu(d) = [n = 1]$ 这个性质，于是式子化为（这里钦定 $n < m$）：

$$
\begin{aligned}
& \sum_{p \in Prime} \sum_{i=1}^{\lfloor \frac{n}{p} \rfloor} \sum_{j=1}^{\lfloor \frac{m}{p} \rfloor} \sum_{d | gcd(i,j)} \mu(d) \\
=& \sum_{p \in Prime} \sum_{d=1}^{n} \mu(d) \lfloor \frac{n}{pd} \rfloor \lfloor \frac{m}{pd} \rfloor
\end{aligned}
$$

令 $pd = t$，则：

$$
\begin{aligned}
& \sum_{p \in Prime} \sum_{d=1}^n \mu(d) \lfloor \frac{n}{t} \rfloor \lfloor \frac{m}{t} \rfloor \\
\end{aligned}
$$

这时候这个和式就可以划分为两个互相独立的部分，所以：

$$
\begin{aligned}
& \sum_{t=1}^n \lfloor \frac{n}{t} \rfloor \lfloor \frac{m}{t} \rfloor \sum_{p | t, p \in Prime} \mu(\frac{t}{p})
\end{aligned}
$$

前面一个和式可以整除分块，后面的可以前缀和，这样我们就做完啦~~~

提交记录：[link](https://www.luogu.com.cn/record/227796033)

## [Luogu-P3455](https://www.luogu.com.cn/problem/P3455) ZAP-Queries

### 题目大意

给定 $a$，$b$ 和 $d$，求：

$$
\sum_{i=1}^{a} \sum_{j=1}^b [gcd(i,j) = d]
$$

### 思路

对于给定的式子，我们和第一题一样做如下变换：

令 $n = \frac{a}{d}, m = \frac{b}{d}$，则

$$
\begin{aligned}
& \sum_{i=1}^n \sum_{j=1}^m [gcd(i,j) = 1] \\
=& \sum_{i=1}^n \sum_{j=1}^m \sum_{k | gcd(i,j)} \mu(k) \\
=& \sum_{k=1}^n \sum_{i=1}^{\lfloor \frac{n}{k} \rfloor} \sum_{j=1}^{\lfloor \frac{m}{k} \rfloor} \mu(k) \\
=& \sum_{k=1}^n \mu(k) \lfloor \frac{n}{k} \rfloor \lfloor \frac{m}{k} \rfloor
\end{aligned}
$$

所以又变回了第一题的整除分块加前缀和了。

提交记录：[link](https://www.luogu.com.cn/record/227796033)

## [Luogu-P3327](https://www.luogu.com.cn/problem/P3327) 约数个数和

### 题目大意

已知函数 $d(n)$ 表示整数 $n$ 的约数个数，给定 $n,m$ 求：

$$
\sum_{i=1}^n \sum_{j=1}^m d(ij)
$$

### 思路

对于函数 $d(n)$，可以发现这是一个积性函数，但是不是一个完全积性函数，所以不能把 $d(ij)$ 拆成 $d(i) \times d(j)$。

但是对于这个形式的函数 $d(n)$，我们可以根据定义发现另一个更优秀的性质：

$$
d(ij) = \sum_{x|i} \sum_{y|j} [gcd(x,y) = 1]
$$

通过枚举原来的 $i,j$，加上容斥，我们就可以把 $d(ij)$ 写开来，这样有利于后面的变换。

$$
\begin{aligned}
    & \sum_{i=1}^n \sum_{j=1}^m d(ij) \\
    =& \sum_{i=1}^n \sum_{j=1}^m \sum_{x|i} \sum_{y|j} [gcd(x,y) = 1] \\
    =& \sum_{i=1}^n \sum_{j=1}^m \sum_{x|i} \sum_{y|j} \sum_{d|gcd(x,y)} \mu(d) \\
    =& \sum_{x=1}^n \sum_{y=1}^m \lfloor \frac{n}{x} \rfloor \lfloor \frac{m}{y} \rfloor \sum_{d|gcd(x,y)} \mu(d) \\
    =& \sum_{d=1}^n \mu(d) \sum_{x=1}^{\lfloor \frac{n}{d} \rfloor} \sum_{y=1}^{\lfloor \frac{m}{d} \rfloor} \lfloor \frac{n}{x} \rfloor \lfloor \frac{m}{y} \rfloor \\
    =& \sum_{d=1}^n \mu(d) (\sum_{x=1}^{\lfloor \frac{n}{d} \rfloor} \lfloor \frac{n}{xd} \rfloor) (\sum_{y=1}^{\lfloor \frac{m}{d} \rfloor} \lfloor \frac{m}{yd} \rfloor )
\end{aligned}
$$

令 $f(x) = \displaystyle \sum_{i=1}^n \lfloor \frac{n}{i} \rfloor$，则原式可以表达为：

$$
\begin{aligned}
\sum_{d=1}^n \mu(d) f(\lfloor \frac{n}{d} \rfloor) f(\lfloor \frac{m}{d} \rfloor)
\end{aligned}
$$

对于 $\mu(d), f(n)$ 均可以预处理得到，剩下的就是整除分块了。

提交记录：[link](https://www.luogu.com.cn/record/227851099)