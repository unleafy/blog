---
title: "2026 Summer Day7"
description: ""
date: "2026-07-23T14:05:22.783Z"
draft: false
showHeroImage: false
tags: ["模拟赛"]
categories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day7

[模拟赛](https://zhengruioi.com/contest/2148)

让我们恭喜 Unleafy 在 ZROI C Day1 中获得了 $25 + 100 + 0 + 15$，rk104 的好成绩！！！

~~依旧屎一样的模拟赛表现~~

可做的分 $100 + 100 + 20 + 40 \to 25 + 100 + 0 + 15$

## T1 合并果子

> 给你一个序列 $a$，你可以进行 $k$ 次操作，每次合并两个相邻的数，新的权值为他们两个之和，求最后 $a$ 的最小值的最大值。
> $\sum n \le 2 \times 10^6$

学数据结构学魔怔了，以为是什么并查集维护合并的连续区间，然后贪心合并，经过 1h 才注意到 **最小值的最大值**，才想到二分答案，然后二分答案的 $check$ 又写错了，获得了并查集 $25pts$ 的好成绩。

后来重新 ~~又又又又又又~~ 写了一遍，赛后过了，依旧不知道赛时为什么 $check$ 写错了。

## T2 旮旯给木

> A 和 B 进行一个博弈。
> A 先手，对于一个数字 $x$ 可以选择一个正整数 $y < x$，然后 $x \leftarrow x \oplus y$，谁不能操作了就输了。
> 问你后手 B 是否可以获胜。 $x \le 10^9$

碰到博弈论的题目，一般就是打表找规律，但是这道题特别良心的给了值域范围内的所有大数据。然后就打开我们的 vscode 进行一个比对，发现只有满足 $x = 2^k$ 的数才满足题目要求，判断即可。

哎哎，神秘博弈论，场上没调过 T1 的我以为开错签到题了 QAQ。

## T3 晚安。

> 定义 $f(x, i)$ 表示 $x$ 在二进制下的第 $i$ 位。
> 
> 定义 $\displaystyle cost(p, q) = \sum_{i} ((f(p, i) \mid f(q, i))x_{i} + (f(p, i) \& f(q, i))y_{i} + (f(p, i) \oplus f(q, i))z_{i})$。
> 
> 给定一个数组 $a$ 求 $\displaystyle \max_{1 \le i \le n, 1 \le j \le n} cost(a_{i}, a_{j})$，有 $q$ 次操作，每次向 $a$ 数组末尾插入一个新值，并输出答案。$\sum n \le 10^6$

这道题特别的毒瘤，但也算是一道挺好的题目。

赛时这道题没有足够的时间打暴力，所以没有分，也是这场比赛的一大失误，没有合理分配好时间。

考虑根号分治，我们不难想到将 $x$ 拆位为低位和高位各 $\frac{h}{2}$，然后发现高位和低位的贡献相互不影响，所以可以分开枚举计算。问题转化为给定 $x$ 的高位 $A$，$y$ 的低位 $B$，找到 $\displaystyle \max(cost(A, a))$ 和 $\displaystyle \max(cost(b, B))$，可以开一个二维数组统计一下。这样计算的话复杂度是 $O(V \sqrt{V})$ 的。

所以我们考虑直接刻画一位一位匹配的过程，我们令 $f_{i, S}$ 表示当前匹配到了第 $i$ 为，$S$ 为 $x$ 的后 $h - i$ 位与 $y$ 的前 $i$ 为合成的一个状态。转移的时候考虑枚举第 $i + 1$ 位的状态，计算 $cost$ 即可。复杂度 $O(V \log V)$。但是插入的维护成本较大。

我们尝试把高低位纳入状态的定义范围，定义 $f_{A, B}$ 表示 $x$ 的高位状态为 $A$ 时，它的低位与 $B$ 能产生的最大贡献。转移 $f_{A, b} \leftarrow \max(f_{A, b}, cost(B, b))$，计算答案的时候就枚举 $y$ 的高位 $a$，答案就是 $\max(cost(A, a) + f_{a, B})$，查询 $O(\sqrt{V})$，总复杂度 $O(V\sqrt{V} + q\sqrt{V})$。

发现这两种做法各有优点，一个预处理快，一个查询修改快，考虑结合一下，用之前的那个 $O(V \log V)$ 的 DP 来预处理，大概长这个样子：

```cpp
  for (int i : ex_highs) {
    for (int j = 0; j < v_low; j++) dp[j] = -INF;
    for (auto low : lows[i]) dp[low] = 0;

    for (int j = 0; j < b_low; j++) {
      long long w01 = x[j] + z[j], w10 = x[j] + z[j], w11 = x[j] + y[j];
      long long p = 1 << j;
      for (int k = 0; k < v_low; k += (p << 1)) {
        for (int t = 0; t < p; t++) {
          int m0 = k + t, m1 = m0 + p;
          long long o0 = dp[m0], o1 = dp[m1];
          long long n0 = -INF, n1 = -INF;

          if (o0 != -INF) {
            n0 = o0; 
            n1 = o0 + w01;
          }
          if (o1 != -INF) {
            long long t0 = o1 + w10; if (t0 > n0) n0 = t0;
            long long t1 = o1 + w11; if (t1 > n1) n1 = t1;
          }
          dp[m0] = n0, dp[m1] = n1;
        }
      }
    }

    for (int j = 0; j < v_low; j++) {
      f[i][j] = dp[j];
      f_T[j][i] = dp[j];
    }
  }
```

复杂度 $O(V \log V + q\sqrt{V})$。

## T4 括号序列

> 给你一个合法的括号序列，求其中有多少个区间 $[l, r]$ 满足翻转这个区间后，原字符串依然是一个合法的括号字符串。$n \le 10^6$。

我们将左括号转化为 $1$，将右括号转化为 $-1$，那么一个合法的括号序列满足任意位置的前缀和 $\ge 0$。

接下来我们考虑判断一个区间 $[l, r]$ 是否满足条件。如下图所示。

![](../assets/2026%20Summer%20Day7/2026-07-23-23-17-20-image.png)

感觉出题人这个思路还是很新颖的，我们将前缀和用曲线刻画出来，发现反转一个区间 $[l, r]$ 实际上就是将这个曲线以 $a[l - 1]$ 和 $a[r]$ 的连线的中点中心对称一下，这样的话为了满足任意时刻前缀和 $\ge 0$，我们可以得到：

$$
a_{l - 1} + a_r - \max_{l - 1 \le i \le r} a_i \ge 0
$$

我们考虑使用分治来计算贡献，对于一个分治中点 $mid$，令 $pre_i$ 表示右半部分的前缀最大值，$suf_i$ 表示左半部分的后缀最大值，那么式子就可以转化为：

$$
a_L + a_R \ge \max (suf_L, pre_R)
$$

把 $\max$ 分类讨论一下就是一个二维偏序问题，可以树状数组维护，但是群友也给出了 $O(1)$ 维护的方法，利用的是每次前缀和波动最多相差 $1$。

## 后记

晚上补 T3 题也是补了很久，怪不得题目名叫 **晚安。**

![](../assets/2026%20Summer%20Day7/2026-07-23-23-27-27-image.png)

也是获得了出题人的认可。

今日还达成成就：最后一个离开机房

![](../assets/2026%20Summer%20Day7/2026-07-23-23-28-40-596DF9E5F65D903322439DD343FBA2BA.jpg)
