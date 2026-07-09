---
title: "July 9th-暑期记录"
description: ""
date: "2026-07-09T12:45:04.645Z"
draft: true
showHeroImage: false
tags: ["Kruskal"]
categories: ["记录"]
series: ["2026 暑期记录"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# July 9th-暑期记录

## P3665 [USACO17OPEN] Switch Grass P

[题目链接](https://www.luogu.com.cn/problem/P3665)

首先发现要求的是 **异色点之间的最短距离**，不难发现最后的答案一定是某一条边的边权。证明就是如果一条路径的起点和终点为异色点，那么必然存在一条边的两个端点颜色不同，所以选择这一条边一定比选择其他几条边的组合更优（因为 $w_i > 0$）。然后这条边一定是在原图的最小生成树上的，我们就可以维护这个结构。

还有为什么这道题这么卡常喵 ₍˄·͈༝·͈˄₎◞ ̑̑

[code](https://www.luogu.com.cn/record/284886164)

~~今天怎么只写了一题~~
