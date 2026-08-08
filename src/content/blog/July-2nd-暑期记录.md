---
title: 'July 2nd-暑期记录'
description: ''
date: '2026-07-02T11:36:50.111Z'
draft: false
showHeroImage: false
tags: ['Kruskal重构树', '交互', '二分图']
categories: ['记录']
series: ['2026 暑期记录']
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# July 2nd-暑期记录

## P9638 「yyOI R1」youyou 的军训

[题目链接](https://www.luogu.com.cn/problem/P9638)

发现题目的意思就是给定一个阈值 $x$, 把图上所有小于 $x$ 的边都断掉, 然后询问 $u$ 所在的连通块大小. 而其题目保证了 **敏感值的相对大小(排名)不会变化**, 所以我们可以想到 _Kruskal_ 重构树, _Kruskal_ 重构树是用来处理 **瓶颈路** 的相关问题的一种树状结构, 同时又具有堆的性质. 我们建出 _Kruskal_ 重构树后, 可以倍增寻找一个深度最深的祖先 $fa$ 满足 $val_{fa} \ge x$, 输出 $fa$ 的大小即可. 对于边权的修改用数组记录即可.

[code](https://www.luogu.com.cn/problem/P9638)

## P5182 棋盘覆盖

[题目链接](https://www.luogu.com.cn/problem/P5182)

一道二分图匹配建模的板子题. 我们有以下结论:

> 任意一张网格图都是可以 2 染色的二分图. 且染色结果和 $i + j$ 的奇偶性相关.

对于这道题, 我们首先知道了这张网格图是一张二分图, 同时所有的骨牌都是 $2 \times 1$ 的, 即如果我们钦定一个点被一张骨牌覆盖, 那么它的四方向联通的位置的其中之一会被这张骨牌覆盖, 这就是一个匹配的模式, 按照这个模式建图即可.

[code](https://www.luogu.com.cn/record/283601551)

## P4768 [NOI2018] 归程

[题目链接](https://www.luogu.com.cn/problem/P4768)

依旧著名题目归程. 题意描述中的**有积水的边一定是海拔相对最低的一些边**可以发现有积水的边满足某种单调性, 而有积水的边又是无法开车的, 所以如果出发后遇到一条有积水的边, 后面的路都需要走过去.

所以建出 _Kruskal_ 重构树, 对于询问节点 $u$, 在 _Kruskal_ 重构树上暴力跳, 找到满足 $val_p \le lim$ 的点. 那么 $p$ 和 $p$ 的子树内的点都是**联通**的, 所以我们找到和 $u$ 联通的点里面距离 $1$ 最近的距离即可, 这个距离可以预处理出来, 然后 _dfs_ 统计到父亲节点上即可.

[code](https://www.luogu.com.cn/record/283614456)

## P15303 『NFC-OI R1』序列陆

[题目链接](https://www.luogu.com.cn/problem/P15303)

~~怎么又是交互题~~

给你一个原序列 $a$, 满足前面若干个为 $0$, 后面若干个为 $1$, 对这个序列中的某一个区间进行取反操作后得到序列 $b$, 你每次可以询问一个区间 $\operatorname{Q}(l, r)$, 交互库会返回 $\sum_{i = l}^{r} b_i$ 的值, 求序列 $b$.

发现取反后的结构一定是 $000 \dots 001111 \dots 111000 \dots 000111 \dots 11111$ 的结构, 考虑如何找到连续段的断点. 首先询问一次 $\operatorname{Q}(1, n)$ 可以得到序列中 $1$ 的个数 (假设为 $all$). 然后询问一次 $\operatorname{Q}(n - all + 1, n - all + 1)$ 可以知道这个断点在 $n - all$ 之前还是之后, 分别对两种情况二分答案就可以知道断点的位置.

这么做的话, 需要对特殊性质特判一下, 随便乱搞都可以.

[code](https://www.luogu.com.cn/problem/P15303)
