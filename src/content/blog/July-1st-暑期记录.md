---
title: "July 1st-暑期记录"
description: ""
date: "2026-07-01T10:47:59.098Z"
draft: false
showHeroImage: false
tags: ["adhoc", "交互"]
categories: ["记录"]
series: ["2026 暑期记录"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# July 1st 暑期记录



## P14511 [NFLSPC #8] 轨道交通



[题目链接](https://www.luogu.com.cn/problem/P14511)



有 $n$ 种不同颜色, 每种颜色有 $n + 1$ 个不同的点, 求每种颜色选两个点, 使得线段交点最少的方案.



首先显然存在一种选择方案, 使得线段两两不交, 考虑如何构造这种方案. 我们先考虑所有点在一条直线的情况, 这种情况直接每次取最短的相同颜色的线段删除前缀即可. 再考虑二维的情况, 我们对原来的点按 $x$ 坐标排序, 就把问题转化到了平面上的问题, 同一维统计即可.



[code](https://www.luogu.com.cn/record/283468326)



## P6892 [ICPC 2014 WF] Baggage



[题目链接](https://www.luogu.com.cn/problem/P6892)



给一个长度为 $2n$ 的 $BA$ 交替的序列, 在左侧有 $2n$ 个空位, 求把原序列变成 $AAA \dots AABBB \dots BB$ 的最小步数和方案.



观察样例, 猜测最小步数为 $n$, 考虑如何构造方案. 依旧通过样例发现, 所有的第一步都是把一个 $AB$ 移到 $-1$ 的位置上, 而且最后几步都是把最后组成的 $AA$ 移至最前面, 然后把前面的 $BB$ 置换到后面来. 发现性质不够充分, 继续手模样例.



$$
\begin{aligned}
x x B A B A B A B \textcolor{red}{A B} A \\
A B B A \textcolor{red}{B A} B A B x x A \\
A B B A x x B \textcolor{red}{A B} B A A \\
A \textcolor{red}{B B} A A B B x x B A A \\
A x x A A B B B B B \textcolor{red}{A A} \\
A A A A A B B B B B x x
\end{aligned}
$$



发现进行了前两步之后, 取区间 $[5, 8]$ 发现和未操作过的序列结构相同, 于是考虑递归找规律. 但是这个方法并不是很好, 虽然有一些很有趣的发现, 但是无法找到通解. 后来结合 xzy 讲题的速录发现, 每次递归子问题时序列长度减少 $8$, 所以需要考虑 $2n \bmod 8$ 的余数的情况, 然后递归求解, 判断即可.



但是 [xzy](https://www.luogu.com.cn/article/okiewu0c) 的思路和我不太一样. xzy 和我一样发现了上面 $ABBAxxBABA \dots BABBAA$ 的结构, 但是他将整个过程分为了两个阶段, 然后直接通过区间个数得出了最后的方案和 $n \bmod 4$ 有关的结论, 还是太强了 Orz.



[code](https://www.luogu.com.cn/record/283493833)



## P15061 琥峪枫



[题目链接](https://www.luogu.com.cn/problem/P15061)



依旧神秘交互题. 给你一棵有 $n$ 个节点, 高度为 $h$ 的满二叉树 (定义根节点的高度为 $1$), 每个节点有一个权值 $f_u$, 同时交互库有一个排列 $p$, 每次你发起询问 $\operatorname{Q}(u, d)$, 交互库会返回 $\sum_{\operatorname{dist}(u, v) = d} f_v$, 你需要在 $2n + 3$ 次询问内得到答案, 同时对每个点的询问次数不超过 $4$ 次.



凭借直觉, 肯定要对每个点先询问一次 $(u, 1)$, 然后我们得到了:



$$
3 \times \sum_{u} f_u - 2 \times \sum_{u \in \textrm{Leaf}} f_u - f_{root}
$$



接下来的问题转化为了如何在 $n + 3$ 次操作内得到 $\sum_{u \in \textrm{Leaf}} f_u$ 和 $f_{root}$. 如果 $root$ 的左右儿子为 $u, v$, 那么 $f_{root}$ 可以表示为 $\operatorname{Q}(u, 1) + \operatorname{Q}(v, 1) - \operatorname{Q}(root, 2)$, 接下来考虑如何在 $n + 2$ 次内求出 $u, v, root$, 发现如果对每一个点再询问一次 $\operatorname{Q}(u, h + 1)$ 的话, 只有 $u, v, root$ 的回答是 $0$, 其他都是非 $0$ 的, 我们可以根据这个得到 $u, v, root$, 然后就是计算答案了.



[code](https://www.luogu.com.cn/record/283539031)



~~所以今天怎么写的都是每日一题(雾🌫)~~
