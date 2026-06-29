---
title: '2025 Summer Day20'
description: '暑期集训 Day20'
date: '2025-08-02T03:22:39.534Z'
draft: false
tags:
  - 字符串
  - 哈希
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---


# 2025 Summer Day20


# 课堂内容

## 字符串哈希

定义哈希函数 $H(s)$:

$$
H(s) = \sum_{i=1}^n base^{n-i} s_i \bmod P
$$

其中 $base$ 大于字符集大小。这个哈希函数的冲突概率为 $\frac{1}{P}$。

通过这个定义，我们可以通过前缀和处理，得到这个字符串上每个区间的哈希值。

## 树哈希 （**重点**）

定义哈希函数 $H(u)$，表示以 $u$ 为根节点的子树的哈希值。

$$
H(u) = \sum_{i} H(son(u, i))^{1 + \sum_{j<i} 2 \times size(son(u, j))} + 1 + 2 \times base^{2 \times size(u) - 1}
$$

其中 $son(u, i)$ 表示 $u$ 的第 $i$ 个儿子，$size(u)$ 表示以 $u$ 为根节点的子树的大小。

## 字典树

字典树就没什么好讲的了，注意一下 0/1 字典树就行。