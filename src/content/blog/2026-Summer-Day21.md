---
title: "2026 Summer Day21"
description: ""
date: "2026-08-06T12:14:20.832Z"
draft: false
showHeroImage: false
tags: ["字符串","AC 自动机"]
categories: ["记录"]
series: ["2026 Summer"]
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day21

## P5357 【模板】AC 自动机

[题目链接](https://www.luogu.com.cn/problem/P5357)

复习了一下板子，突然发现距离我学了 *AC* 自动机但是没过 S T3 在考场上猛锤大腿已经过去快一年了((

但是板子也写错了很多地方。比如 $fail_{v} = tr_{fail_u, c}$ 而不是 $fail_v = fail_u$，因为还要再向后匹配一个节点。

[code](https://www.luogu.com.cn/record/291057008)

## P2444 [POI 2000 R1] 病毒

[题目链接](https://www.luogu.com.cn/problem/P2444)

如果我们将所有的危险病毒串放到一个 *ACAM* 上的话，那么题目的条件就转化为了在 ACAM 上找到一条从根出发的路径，经过一个环，且没有任何一个被打了标记的点在这个路径上。

我们在 *ACAM* 构建 $fail$ 指针的时候，将标记沿着 $fail$ 指针的路径传递，因为 $fail$ 指针的定义是满足状态 $v$ 是状态 $u$ 的最长后缀的状态 $v$，然后跑 $dfs$ 找环即可。

[code](https://www.luogu.com.cn/record/291087561)

## P3041 [USACO12JAN] Video Game G

[题目链接](https://www.luogu.com.cn/problem/P3041)

发现是模式串匹配问题，所以先将所有模式串扔到一个 *ACAM* 上，然后考虑如何 *DP*。

我们可以令 $dp_{i, j}$ 表示当前操作序列的长度为 $i$，且走到了 *ACAM* 上的状态 $j$ 的最大价值。那么转移的时候考虑枚举下一个字符 $c$：

$$
dp_{i+1, trans(j, c)} = \max(dp_{i, j}, dp_{i, j} + val(trans(j, c))
$$

最后的答案就是 $\sum dp_{k, i}$。

[code](https://www.luogu.com.cn/record/list?pid=P3041&user=1023191)

## P4052 [JSOI2007] 文本生成器

[题目链接](https://www.luogu.com.cn/problem/P4052)

将答案通过容斥转化为 $26^m - cnt$，其中  $cnt$ 表示所有合法文本中不包含任何可识别子串的方案数。

这个 *DP* 的形式和上面一题非常类似，我们令 $dp_{i, j}$ 表示当前文本串长度为 $i$，且走到了 *ACAM* 的状态 $j$，不经过任何可识别状态的方案数。将标记沿着 $fail$ 指针传递，然后每次枚举不存在标记的状态 $trans(j, c)$ 转移：

$$
dp_{i + 1, trans(j, c)} += dp_{i, j}
$$

最后的答案就是 $26^m - \sum dp_{m, i}$，注意模数为 $10^{\color{red}4} + 7$。

[code](https://www.luogu.com.cn/record/291158671)
