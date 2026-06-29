---
title: '2025 Summer Day24'
description: '暑期集训 Day24'
date: '2025-08-05T03:22:39.534Z'
draft: false
tags:
  - 比赛
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---


# 2025 Summer Day24


# Problem-A [IEEE 754](https://zhengruioi.com/problem/3281?cid=1952)

## 题目描述

题目背景告诉你浮点数表示法(IEEE 754 标准)，要求你求 $5^n$，其中 $n < 1024$。

## 思路

赛场上是直接写的高精度，但是赛后看过题解发现有更简单的做法。

首先用 *Python* 计算发现，$5^1023$ 大约有 800 位，而 *double* 类型的存储精度有 308 位，所以可以用 *double* 类型计算，但是要计算的是 $5^(-n)$，因为是浮点数。

提交记录：[Link](https://zhengruioi.com/submission/928510)

# Problem-B [探险](https://zhengruioi.com/problem/3282?cid=1952)

## 题目描述

给定一个数组 $a$，有两个人轮流操作，每次交换一对 $i,j$，谁进行操作后数组 $a$ 有序（从小到大），则这个人获胜。同时有 $q$ 个修改操作，分为两类：
+ $1$ $i$ $j$：将 $a_i$ 与 $a_j$ 交换。
+ $2$ $k$：将 $a$ 循环位移 $k$ 次。
你需要对于初始状态和每一个修改后的状态，判断是先手获胜还是后手获胜。

## 思路

赛场上想到了逆序对，但是时间比较少，只剩 $15$ 分钟了，而且最后开的这道题，所以脑子不太好使，就没有想出来。

赛后发现赛时的思路大致是对的，但是没有继续思考下去。容易发现答案和逆序对个数的奇偶性有关，具体来说：
+ 当逆序对个数为 **奇数** 时，先手获胜；
+ 当逆序对个数为 **偶数** 时，后手获胜
所以问题落在了如何处理修改后的逆序对变化。

对于第一个操作是好求的，因为交换两个数，逆序对个数的奇偶性就会变化。而对于第二个操作，可以发现每一个跨过 $k$ 的两个数的逆序对都会反过来，所以对答案的影响就是 $k(n-k) \bmod 2$（赛时就是这里没有想出来）。

提交记录：[Link](https://zhengruioi.com/submission/929704)

# Problem-C [帕奇欧](https://zhengruioi.com/problem/3283?cid=1952)

## 题目描述

初始时，你手上有 1 张 A 类牌和 1 张 B 类牌。每一回合你都可以从拥有的牌中等概率选取一张，然后获得一张与抽取出来的牌同类型的牌，问你经过无限轮操作后，拥有 $x$ 张 A 类牌和 $y$ 张 B 类牌的概率，对 $10^9 + 7$ 取模。

## 思路

手玩一下样例可以发现，答案为 $\frac{(x + y - 3)! * (x + y - 2)}{(x + y - 1)}$ (这是赛时没化简得结果)。

化简后是 $\frac{1}{x + y - 1}$。

提交记录：[Link](https://zhengruioi.com/submission/927967)

# Problem-D [比赛](https://zhengruioi.com/problem/3284?cid=1952)

## 题目描述

有一种比赛，每一次比赛有若干个小节，率先得到 $M$ 分得人赢下这个小节。给定 $x,y,M$，求最后总分为 $x,y$ 时不同得小节数。

## 思路

赛时推了下式子，没推出来 TAT。

首先可以发现的是，满足题目要求得小节数是一段连续的区间。

所以我们可以分别找到这个区间的上界和下界。容易发现上界为 $\lfloor \frac{x}{M} \rfloor + \lfloor \frac{y}{M} \rfloor$。而下界通过分析可以发现是 $\max(\lceil \frac{x - tx}{M} \rceil, \lceil \frac{y - ty}{M} \rceil, \lceil \frac{x + y}{M - 1} \rceil)$，其中 $tx = \lfloor \frac{x}{M} \rfloor, ty = \lfloor \frac{y}{M} \rfloor$。

提交记录：[Link](https://zhengruioi.com/submission/929782)

# Problem-E [三目运算符](https://zhengruioi.com/problem/3285?cid=1952)

## 题目描述

给定一个只包含 $0,1,x,?,:$ 的合法三目运算符表达式，你可以将其中的 $x$ 替换为 0/1，求所有表达式的值的和对 $10^9 + 7$ 取模的结果。

## 思路

赛时直接枚举 $x$ 的取值，竟然有 *80pts* 的暴力分，出题人还是太良心了。

正解是考虑和表达式树一样的结果，即对于每个节点，都有走向 0/1 的两条路，问题转化为了走向值为 1 的叶子节点的概率。

提交记录：[Link](https://zhengruioi.com/submission/929949)

# Problem-F [卡牌游戏](https://zhengruioi.com/problem/3286?cid=1952)

## 题目描述

你有三种不同类型的卡牌，其中每种卡牌的数量由给定的字符串决定。同时你拥有两个数组 $A,B$，你需要从卡牌中选取恰好 $k$ 张，设你选择的三种卡牌的数量分别为 $x,y,z$，则你的得分为 $A_x \times B_y \times 2^z$。求最大得分。

## 思路

赛时直接枚举前两种卡牌的个数，得了 20 分。

可以发现第三种卡牌的贡献最大。设最多能选 $t$ 张第三种卡牌，则所有 $z < t-60$ 的方案均更差。即使 $A_x,B_y$ 的值从 $10^9 \times 10^9$ 变为了 $1 \times 1$，即变为原来的 $\frac{1}{10^{18}}$，而 $2^60 > 10^{18}$，所以变化后的结果依然更优，所以只要考虑 $t - 60 \le z \le t$ 的方案即可。

提交记录：先欠着吧。