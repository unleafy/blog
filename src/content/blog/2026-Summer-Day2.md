---
title: '2026 Summer Day2'
description: ''
date: '2026-07-18T00:50:26.003Z'
draft: false
showHeroImage: false
tags: ['李超线段树']
categories: ['记录']
series: ['2026 Summer']
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day2

## 李超线段树

李超线段树是维护线段（直线）的数据结构。

李超线段树每个节点维护的是区间 $[l, r]$ 中包含的所有线段在中点 $mid$ 处的最大值。对于一个修改的线段 $k_1x + b_1$, 我们算出他们在中点的函数值 $y_1, y_2$，如果 $y_2 < b_1$，则将这个线段和线段树上的线段交换，保证当前节点维护的信息是正确的，然后考虑更劣的线段下传。

下传的时候判断这条线段在左端点和右端点的函数值的大小来决定下传的方向。

模板：[P4097 【模板】李超线段树 / [HEOI2013] Segment](https://www.luogu.com.cn/problem/P4097)
code: [link](https://www.luogu.com.cn/record/286917510)

## P3521 [POI 2011] ROT-Tree Rotations

[题目链接](https://www.luogu.com.cn/problem/P3521)

这道题就是用权值线段树合并维护左右交换与不交换所获得的逆序对数，每次取最小值借款。

[code](https://www.luogu.com.cn/record/286990965)
