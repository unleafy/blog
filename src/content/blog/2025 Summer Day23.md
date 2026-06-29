---
title: '2025 Summer Day23'
description: '暑期集训 Day23'
date: '2025-08-04T03:22:39.534Z'
draft: false
tags:
  - 交互
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---


# 2025 Summer Day23


# 题目

## [CodeForces-P1672E](https://codeforces.com/problemset/problem/1672/E) notepad.exe

### 题目大意

交互题。
有 $n$ 个长度分别为 $l_i$ 的单词，要求将这些单词排放在一个记事本当中，每两个单词之间需要空格（换行不需要）。你最多可以询问 $n+30$ 次，每次可以询问一个宽度 $w$，裁判会告诉你至少需要高度为 $h$ 的记事本才可以放下所有的单词，求 $\min\{ hw \}$。

### 思路

考虑先用 30 次找出将所有单词放在一行所需的最少宽度 $w_0$，然后对于每一个 $1 \le i \le n$，询问排列成 $i$ 行需要的最少行数，询问次数正好 $n + 30$ 次。

提交记录：[Link](https://vjudge.net/solution/62683276)

## [CodeForces-1010B](https://www.codeforces.com/problemset/problem/1010/B) Rocket

### 题目大意

交互题。
你可以问裁判 60 次问题，裁判会根据一个循环节长度为 $n$ 的数组回答你的问题：如果 $a_i = 0$，则第 $i$ 次回答的是假话；如果 $a_i = 1$，则第 $i$ 次回答的是真话。你并不知道这个数组 $a$。你需要猜一个数 $x (1 \le x \le m)$。每次询问你可以问裁判一个数，他会回答你这个数是 **大于(1)**、**等于(0)**、**小于(-1)**（遵守之前的规则）。

### 思路

首先范围 $m \le 10^9 < 2^{30}$，所以我们可以先用 30 次机会，每次询问 1，就可以得到数组 $a$。然后二分答案 $x$ 就可以了，总询问次数为 60 次。

提交记录：[Link](https://vjudge.net/solution/62685613)

## [Luogu-P1337](https://www.luogu.com.cn/problem/P1337) 平衡点 / 吊打 XXX

### 题目大意

求 $n$ 个点的带权费马点。

### 思路

考虑使用模拟退火，随机检查，剩下的就交给阳寿吧（WA $\times$ 6）。

提交记录：[Link](https://www.luogu.com.cn/record/list?pid=P1337&user=1023191)

## [Luogu-P5285](https://www.luogu.com.cn/problem/P5285) [十二省联考] 骗分过样例

写这道题上来只是小小的心灵受到了大大的震撼。怎么做到再 $20$ 组数据中找规律的啊！