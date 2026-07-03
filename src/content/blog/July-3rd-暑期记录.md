---
title: "July 3rd-暑期记录"
description: ""
date: "2026-07-03T12:08:15.744Z"
draft: false
showHeroImage: false
tags: ["二分图", "网络流", "圆方树"]
categories: ["记录"]
series: ["2026 暑期记录"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# July 3rd 暑期记录



> 中考查分, 698 ~~寄~~



## P2065 [TJOI2011] 卡片



[题目链接](https://luogu.com.cn/problem/P2065)



第一眼看过去以为是二分图最大匹配的板子题, 交了两发 *70pts*, 才发现数据保证的不是 $\sum n, m \le 500$, 而是 $n, m \le 500$, 所以要考虑优化.



发现原图最坏的情况是有 $mn$ 条边的, 考虑如何优化掉边的数量. 又联想到如果 $a = \prod p_i^{\alpha_i}, b = \prod p_i^{\beta_i}$, 那么 $\gcd(a, b) = \prod p_i^{\min(\alpha_i, \beta_i)}$, 而且 $p_i > 1$, 所以我们可以对原来的数据进行质因数分解, 然后分别将他们和 $p_i$ 连容量为 $1$ 的边, 再建立源点和汇点, 向他们连容量为 $1$ 的边, 然后跑最大流就可以了.



[code](https://www.luogu.com.cn/problem/P2065)



## P10932 Freda的传呼机



[题目链接](https://luogu.com.cn/problem/P10932)



~~P5236 双倍经验~~. 还是简单说一下思路. 发现 $C$ 的限制条件最强, 且按照 $C$ 的限制来看, 这张图就是一张仙人掌图. 我们对这张仙人掌图建立圆方树, 然后两点间的距离就映射到了圆方树上的树的距离. 如果 $\operatorname{LCA}(u, v)$ 是圆点, 那么答案就是 $dist_u + dist_v - 2 \times dist_{\operatorname{LCA}(u, v)}$, 但是如果 $\operatorname{LCA}(u, v)$ 是方点的话, 需要单独考虑. 设 $u$ 到 $\operatorname{LCA}(u, v)$ 的路径上经过的最后一个圆点是 $u'$, $v$ 同理设为 $v'$, 那么答案就是 $dist_u - dist_{u'} + dist_v + dist_{v'}$ 再加上 $u'$ 和 $v'$ 在环上的距离. 在建立圆方树的时候记录一下环上的信息, 做一遍前缀和, 输出的时候就可以直接统计了.



[code1](https://www.luogu.com.cn/record/283676726) [code2](https://www.luogu.com.cn/record/283326244) ~~虽然是一样的((~~
