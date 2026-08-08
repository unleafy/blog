---
title: '2026 Summer Day22'
description: ''
date: '2026-08-07T06:53:40.537Z'
draft: false
showHeroImage: false
tags: ['字符串']
categories: ['记录']
series: ['2026 Summer']
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day22

## P3809 【模板】后缀排序

[题目链接](https://www.luogu.com.cn/problem/P3809)

发现还是忘了很多的。

我们用倍增的思想来处理 $rk_i$ 和 $sa_i$，每次倍增地枚举长度 $k$，用二元组 $(rk_i, rk_{i + k})$ 来排序，就可以对子串进行排序。用计数排序可以将复杂度降低至 $O(n \log n)$。

[code](https://www.luogu.com.cn/record/291294982)

## P4248 [AHOI2013] 差异

[题目链接](https://www.luogu.com.cn/problem/P4248)

可以直接将题目中的式子转化为：

$$
\frac{(n-1)n(n+1)}{2} + \sum_{i, j} \operatorname{LCP}(s[i...n], s[j...n])
$$

后面的 $\operatorname{LCP}(s[i...n],s[j...n])$ 可以通过处理 $height$ 数组，并用单调栈处理最值区间解决。复杂度 $O(n \log n )$。

[code](https://www.luogu.com.cn/record/291328310)

## P2408 不同子串个数

[题目链接](https://www.luogu.com.cn/problem/P2408)

之前写过了，还是整理一下，有点忘记了。

使用后缀数组。由于已经将后缀排过序了，那么对于每一个 $\operatorname{LCP}(sa_{rk_i}, sa_{rk_{I-1}})$，都已经由 $sa_{rk_{i-1}}$ 统计过了。所以答案就是 $\frac{n(n-1)}{2} - \sum height_i$。

[code](https://www.luogu.com.cn/record/197041156)
