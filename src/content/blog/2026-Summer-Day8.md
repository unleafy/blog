---
title: '2026 Summer Day8'
description: ''
date: '2026-07-25T11:23:06.892Z'
draft: false
showHeroImage: false
tags: ['DP']
categories: ['记录']
series: ['2026 Summer']
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day8

## P4563 [JXOI2018] 守卫

[题目链接](https://www.luogu.com.cn/problem/P4563)

题目大概就是给你一个高度序列 $h_i$，你可以在一些位置 $p$ 上安插守卫，如果 $t < p$ 且 $(t, h_t)$ 和 $(p, h_p)$ 的连线不被任何山峰阻挡，则在 $p$ 的守卫可以看到位置 $t$，求让所有位置都被监视的最小守卫数量。

令 $dp_{i, j}$ 表示区间 $[i, j]$ 内满足条件的最小守卫数量。通过观察，我们不难发现以下性质：

- 设位置 $r$ 能看到的位置为 $pos_i$，则 $pos_i$ 和 $r$ 的连线的鞋履一定小于 $pos_{i + 1}$ 和 $r$ 的连线。

- 区间 $[i, j]
  $ 要满足条件，则位置 $j$ 上一定要放置守卫。

- $[pos_i, pos_{i + 1} - 1]$ 是一个子问题。

这样我们就可以转移了：

$$
dp_{i, j} = \min_{i \le k < j} \min(dp_{i, pos_i - 1}, dp_{i, pos_i}) + dp_{pos_i + 1, r}
$$

然后我们只需要变换内存能循环的枚举顺序就可以通过这道题了。复杂度 $O(n^2)$。

[code](https://www.luogu.com.cn/record/288259864)

## P10967/P4767 [IOI 2000] 邮局

[题目链接（原版）](https://www.luogu.com.cn/problem/P10967) [题目链接（加强版）](https://www.luogu.com.cn/problem/P4767)

一个很显然的 _DP_ 状态定义是令 $dp_{i, j}$ 表示在前 $i$ 个村庄中放置 $j$ 个邮局获得的最小距离和。转移也很显然：

$$
dp_{i, j} = \min_{1 \le k \le i} dp_{k, j - 1} + cost(k + 1, j)
$$

这里的 $cost(k + 1, j)$ 表示在区间 $[k + 1, j]$ 中放置一个邮局获得的最小距离和，可以预处理，复杂度 $O(n^3)$，可以通过原版题目。

加强版的数据范围扩大到了 $n \le 5000$，世界 _DP_ 显然过不了，需要考虑优化。

其实可以感性理解一下，如果原本的状态为 $(i, j)$，我们现在新增一个邮局 $(i, j + 1)$，那么这个答案显然是会小于 $(i, j)$ 的，同理，如果我们由状态 $(i, j)$ 转移至 $(i + 1, j)$，这个答案显然是会大于 $(i, j)$ 的，所以这里我们就有一个类似单调性的性质，每次转移的时候记录一下转移的位置，下一次转移的时候从这个位置开始，复杂度 $O(n^2)$。大概长这样：

```cpp
  for (int i = 1; i <= P; i++) {
    from[V + 1][i] = V;
    for (int j = V; j >= 1; j--) {
      int p = 0;
      for (int k = from[j][i - 1]; k <= from[j + 1][i]; k++) {
        if (dp[k][i - 1] + w[k + 1][j] < dp[j][i])
          dp[j][i] = dp[k][i - 1] + w[k + 1][j], p = k;
      }
      from[j][i] = p;
    }
  }
```

[code](https://www.luogu.com.cn/record/288349977)

后来发现这个是 **四边形不等式**，不过还没太搞懂。<mark>_TODO_</mark>
