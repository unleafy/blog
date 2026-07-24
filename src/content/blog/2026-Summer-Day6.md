---
title: "2026 Summer Day6"
description: ""
date: "2026-07-22T12:30:34.762Z"
draft: false
showHeroImage: false
tags: ["虚树", "可持久化"]
categories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day6

## 练习

### P5283 [十二省联考 2019] 异或粽子

[题目链接](https://www.luogu.com.cn/problem/P5283)

这道题其实就是找到区间异或和最大的前 $2k$ 个之和，随后答案除以二，这样算可以省略一些复杂的讨论。我们可以套路地使用前缀异或和，将区间 $[l, r]$ 的异或和转化为 $pre_{r} \oplus pre_{l - 1}$，然后使用 0-1 Trie 将这些值存储起来。

维护一个大根堆，存放 $(id, rk, val)$ 表示当前以位置 $id$ 为端点的第 $rk$ 大的区间异或至为 $val$。然后每次取堆顶的维护一下 $(id, rk + 1, val')$ 即可。类似线段树二分的思路。

[code](https://www.luogu.com.cn/record/287787016)

### P2839 [国家集训队] middle 

[题目链接](https://www.luogu.com.cn/problem/P2839)

显然这道题的答案是单调的，我们可以先二份答案 $mid$，对于有小于 $mid$ 的数全部复制为 $1$，对于所有大于 $mid$ 的值全部复制为 $-1$，$check$ 的时候判断区间内是否存在一个字区间满足区间和 $\ge 0$ 的，线段树维护区间和、前缀最大字段和、后缀最大字段和即可。

[code](https://www.luogu.com.cn/record/287843356)
