---
title: '2025 Summer Day15'
description: '暑期集训 Day15'
date: '2025-07-29T03:22:39.534Z'
draft: false
tags:
  - 数学
  - 组合计数
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day15


**Content**：组合数学

**Date**：2025.7.31



# 课堂内容

## 容斥原理

其主要思想为：把禁止违反哪些规则改为钦定违反了哪几条规则，并赋予 $(-1)^k$ （即违反 $k$ 条规则）的容斥系数。

## 二项式反演

$$
\begin{aligned}
g(n) &= \sum_{i=0}^n \binom{n}{i} f(i) \Leftrightarrow f(n) = \sum_{i=0}^n (-1)^{n-i} \binom{n}{i} g(i) \\
g(n) &= \sum_{i=n}^N \binom{i}{n} f(i) \Leftrightarrow f(n) = \sum_{i=n}^N (-1)^{i-n} \binom{i}{n} g(i)
\end{aligned}
$$

# 例题

## [Luogu-P1450](https://www.luogu.com.cn) [HAOI2018] 硬币购物

### 题目大意

给定硬币面值 $c_1, c_2, c_3, c_4$ 和硬币个数 $d_1, d_2, d_3, d_4$，求恰好凑齐 $s$ 元的方案数。$T$ 组询问。

### 思路

首先我们可以对硬币做完全背包，这样我们就知道了用这些硬币可以凑出那些。

然后带上限制条件，做容斥即可。

提交记录：[link](https://www.luogu.com.cn/record/227971787)

## [AGC005D](https://atcoder.jp/contests/agc005/tasks/agc005_d) ~K Perm Counting

### 题目大意

给定 $N,K$，求所有满足 $|a_i - i| \ne k$ 的长度为 $N$ 的排列的个数。

### 思路

参考 Dreamunk 大佬的[题解](http://luogu.com.cn/article/u5s75fn2)。将所有的不合法的情况之间连边，我们就得到了若干条不合法的链，对这些链进行 DP 后，就可以容斥了。

提交记录：[link](https://atcoder.jp/contests/agc005/submissions/68064248)


最后的最后，还是要说一句 ~~我恨计数~~