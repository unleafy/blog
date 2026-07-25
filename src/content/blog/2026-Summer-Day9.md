---
title: "2026 Summer Day9"
description: ""
date: "2026-07-25T14:59:53.307Z"
draft: false
showHeroImage: false
tags: ["DP"]
categories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day9

## 课堂内容

### MOPE 卷积

目前还没搞懂，有空回来补一下。<mark>  *TODO*  </mark>



## 练习

### P4242 树上的毒瘤

[题目链接](https://www.luogu.com.cn/problem/P4242)

给定一棵 $n$ 个节点的树，每个节点有一个颜色 $c_i$，定义两个节点之间的权值 $T(u, v)$ 为两个节点路径上颜色段的数量。有两种操作：

+ 修改 $(u, v)$ 路径上的所有颜色为 $c$。

+ 给定 $m$ 个点，查询 $w_i = \sum_{j \in S} T(i, j)$，其中 $S$ 为给定的点集。

看到那个给定点集，显然是一个虚树题的明显标志。而前面的路径覆盖也很好处理，只需要树剖 + 线段树维护即可。

对于最后一个式子，我们可以考虑使用换根 *DP* 来求。我们令 $f_u$ 表示 $u$ 的子树内所有节点的 $T(u, v) - 1$ 的权值之和。令 $g_u$ 表示 $u$ 道整棵树的 $T(u, v) - 1$ 之和。转移如下：    

$$
\begin{aligned}
& f_u = \sum_{v \in son(u)} f_v + siz_v \times (T(u, v) - 1) \\
& g_v = g_u + (m - 2 \times siz_v) * (T(u, v) - 1)
\end{aligned}
$$

其实就是朴素的换根 *DP* 的思路。这道题也是思路 30 mins，代码 *inf* 的毒瘤题目，管不得叫 ~~树上的毒瘤~~。

1. 树剖路径 *query* 的时候要注意，*swap* 之后会对最后统计的答案有影响，需要写分类讨论，活着将统计答案的变量也 *swap* 了。

2. 路径 *query* 最后 $u, v$ 在同一条链上的时候，`res.lv` 表示的是靠上面那部分的值，而并非 `res.rv`。

3. 虚树建树的时候，最好新靠一个 `vector` 来存储关键节点，不要在原来的数组上操作。

4. 注意虚树建边的下标，通常写成 `LCA(key[i - 1], key[i])` 的情况下，连边是 $(key_i, LCA(key_{i - 1}, key_i)$。

5. `dp1` 后需要清空当前节点的数组，不可以在每次查询前 `memset`，否则复杂度会退化成 $O(nm)$，需要手动清理。

6. 转移方程中的 $siz_v$ 统计的是 $v$ 的子树中的关键点的数量，由于虚树会引入非关键点的 *LCA*，所以需要对输入的关键点打标记来统计 $siz_v$。

[code](https://www.luogu.com.cn/record/288564959)

### P3413 SAC#1 - 萌数

[题目链接](https://www.luogu.com.cn/problem/P3413)

用这道题来复习一下数位 *DP*，也很久没有写了。

这道题多写几个数就不难发现，**一个回文串包含 $aa$ 活着 $aba$ 模式的子串。**

使用记忆化搜索的方式，每次递归 `dfs(pos, p1, p2, f, lim, lead)` 表示当前枚举到了数位的第 $pos$ 位，前一位和前两位分别为 $p1$ 和 $p2$，$f$ 表示当前数字是否已经满足要求，直接枚举下一位的数字转移即可，

[code](https://www.luogu.com.cn/record/288578684)
