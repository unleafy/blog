---
title: "July 8nd 暑期记录"
description: ""
date: "2026-07-08T12:24:01.966Z"
draft: false
showHeroImage: false
tags: ["adhoc"]
categories: ["记录"]
series: ["2026 暑期记录"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# July 8nd 暑期记录

> 前几天去上海了, ~~缺了几天没写题~~

## P15088 [UOI 2025 II Stage] Digital Game

[题目链接](https://www.luogu.com.cn/problem/P15088)

一道博弈论的题目. 最开始认为和 **相同数字之间的奇偶性相关**, 但是发现这个性质好像也不太可做, 因为操作的时候可以删除某些数字, 使得这个距离是动态变化的. 

后来看了看题解, 发现一个极其神秘的性质:

> 当 $n \ge 20$ 时, Us 必胜.

~~不是这是什么鬼性质啊, 这么神秘~~

题解的证明也没太看懂, 感性证明一下. 定义同 [Turkey_VII](https://www.luogu.com.cn/article/etuoy535) 的题解.

首先 Vus 只可能在整个串删空的情况下才能获胜, 所以他一定要使 $d$ 变小, 才能更大概率地让 Us 失败. 而 Us 获胜的临界条件也就是 $\operatorname{cnt}(l, r) \times 2 \le len$, 对于 $n < 20$ 的部分暴搜即可.

[code](https://www.luogu.com.cn/record/284564582)

## P15304 『NFC-OI R1』序列拾

[题目链接](https://www.luogu.com.cn/problem/P15304)

众所周知, 集合并的限制比集合交的限制更少, 所以我们将交集 $T_i \cap T_j$ 转化为 $\complement_S (\complement_S T_i \cup \complement_S T_j)$ 然后问题就转化为了使得 $\sum_i |T_i|$ 最小, 然后又不会做了. 去看题解发现, 答案一定有一个子结构是选取 $\{ 1, 2, \dots, p\}$ 的所有子集, 然后在考虑剩下的 $q$ 个怎么取, 发现是子问题, 递归处理即可.

[code](https://www.luogu.com.cn/record/284654519)
