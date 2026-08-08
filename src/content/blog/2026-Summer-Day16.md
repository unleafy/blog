---
title: '2026 Summer Day16'
description: ''
date: '2026-08-01T06:51:28.371Z'
draft: false
showHeroImage: false
tags: ['数学', '生成函数']
categories: ['记录']
series: ['2026 Summer']
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day16

## CF1097D Makoto and a Blackboard

[题目链接](https://codeforces.com/problemset/problem/1097/D)

给定两个数 $n, k$，要求恰好操作 $k$ 次，每次选择 $n$ 的一个因子，并将 $n$ 变为 $n / d$，求最后剩下的数字的期望。

我们考虑最朴素的方法，定义 $dp_{i, j}$ 表示第 $i$ 次操作后黑板上的数字为 $j$ 的概率。答案即为 $\prod{i} dp_{k, i} * i$，但是这样做的复杂度是 $O(n^2m)$ 的，显然在大数据范围下过不了。

我们考虑优化，由于 $n$ 在 $10^{15}$ 下的因数个数很大，但是可以考虑将 $n$ 质因数分解，因为质数的个数范围相对较小。然后我们对于每一个质数的幂次分别计算概率。

[code](https://codeforces.com/problemset/submission/1097/385063882)

## Coin

[题目链接](https://vjudge.net/contest/837261#problem/G)

这道题好像是 Atcoder FPS 24 题里面的一题吧。

也是通过这道题了解了一下可撤销背包。简单来说就是维护一个类似于滑动窗口的东西，每次有位置超过窗口范围后，就把背包的过程反着做一遍，就把这个位置的贡献消除了，复杂度 $O(nm)$。

[code](https://vjudge.net/solution/71562620)

## F - Yes or No

[题目链接](https://atcoder.jp/contests/agc019/tasks/agc019_f?lang=en)

发现最后的答案不劣于 $\max(n, m)$，但是考虑在这组数据下：

```plain
1 1
```

最后的答案 $1.5$，因为你有 $\frac{1}{2}$ 的概率猜对第一个位置是什么，而第二个位置猜对的概率为 $1$，所以最终期望为 $1.5$。

相较于 $\max(n, m)$，这个样例多了什么呢？发现当 $n = m$ 的时候，当前位置猜对的概率一定为 $\frac{1}{2}$，所以最后的答案是 $\max(n, m) + \frac{1}{2} T$，其中 $T$ 的含义为：

> 在一个二维平面内，每次可以向下或者向右走一步，最终中点为 $(n, m)$，走过直线 $y=x$ 的期望次数。

[code](https://atcoder.jp/contests/agc019/submissions/78064924)
