---
title: '2025 Summer Day9'
description: '暑期集训 Day9'
date: '2025-07-23T03:22:39.534Z'
draft: false
tags:
  - DP
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day9


**Content**：DP (state, num)

**Date**：2025.7.25



# 概览

+ 数位 DP
+ 状压 DP

# 内容

## 状压 DP

状压 DP，即状态压缩 DP，通常情况下和子集问题挂钩，以二进制的第 $i$ 位表示集合中的每个子集选还是不选。

有一种 $O(3^n)$ 的子集枚举方式：

**Code**

```cpp
for (int i = 0; i < (1 << n); i++) {
    for (int j = i; j; j = (j - 1) & i) {
        // * j 即为 i 的子集
    }
}
```

## 数位 DP

数位 DP，即不是以数字为状态进行动态规划，而是和人一样，对于数字的每个数位作为状态，常见问题为：


**常见问题：**
给定区间 $[L,R]$，求出在这个区间中满足某种条件的数的个数。


# 例题

## [洛谷-P1896](https://www.luogu.com.cn/problem/P1896) 互不侵犯

### 思路

我们定义 $dp_{i,s,j}$ 表示第 $i$ 行的放置状态为 **二进制表示下的** $s$ 时的放置 $j$ 个国王的方案数。

首先根据题意，我们要排除一些不合法的放置状态：

+ 对于 $\forall s$，如果 $s \ \& \ s << 1$ 或 $s \ \& \ s >> 1$ 不为 $0$，那么状态 $s$ 不合法。（不能左右相邻）
+ 对于任意相邻两行的状态 $\forall s,t$，如果 $s \ \& \ t$ 或 $s \ \& \ t << 1$ 或 $s \ \& \ t << 1$ 不为 $0$，那么状态 $s,t$ 不合法。（题目要求）

然后对于转移就是对于所有合法状态的累加。
提交记录：[link](https://www.luogu.com.cn/record/226743063)

## [洛谷-P3959](https://www.luogu.com.cn/problem/P3959)

### 思路

容易发现最后的答案和深度有关，不放就以深度作为状态。

定义 $dp_{d,s}$ 表示当前树的深度为 $d$，在树上的节点的状态为 $s$。

转移就是枚举 $s$ 的子集 $t$，方程如下：

$$
dp_{d,s} = min_{t \in s} \{dp_{d-1,t} + d \times cost_{t,s}\}
$$

其中 $cost_{t,s}$ 表示从集合 $t$ 变为集合 $s$ 的最小花费，可以预处理得到。

提交记录：[link](https://www.luogu.com.cn/record/226771088)

## [洛谷-P4363](https://www.luogu.com.cn/problem/P4363) 一双木棋 chess


### 思路

轮廓线 DP。

我们以一个长度为 $n+m$ 的二进制串表示轮廓线，其中如果第 $i$ 位为 $0$，则向下走，否则向右走。手搓一下样例可以发现状态的转移是将二进制串中的 $01$ 变为 $10$。直接转移不好转移，但是可以记忆化搜索。

定义 $dp_{s}$ 表示轮廓线的状态为 $s$ 下的答案。深搜 + 记忆化即可。

提交记录：[link](https://www.luogu.com.cn/record/226844106)

## [洛谷-P4371](https://www.luogu.com.cn/problem/P4371) 花神的数论题

### 思路

我们发现直接在十进制下做状态不好表示，而题目的答案和二进制的乘积相关，不妨就直接以二进制表示状态。定义 $dp_{i,j,k}$ 表示当前枚举到了二进制下的第 $i$ 位，数字最后在二进制下有 $j$ 个 $1$，目前已经选择了 $k$ 个 $1$ 的方案数。

答案即为 $\Large \prod_{i=1}^{\log_2n} i^{dp_{\log_2 n,i,0}}$。

**注意**：统计 $dp$ 数组的时候不可以取模（也不会爆 *long long*）

提交记录：[link](https://www.luogu.com.cn/record/226830688)