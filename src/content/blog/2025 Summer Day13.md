---
title: '2025 Summer Day13'
description: '暑期集训 Day13'
date: '2025-07-27T03:22:39.534Z'
draft: false
tags:
  - 数据结构
  - 平衡树
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day13


**Content**：平衡树

**Date**：2025.7.29



# 具体内容

## Leafy Tree 和 Un-leafy Tree

*Leafy Tree*：表示将所有的数据存放在叶子节点的树形数据结构，类似 **线段树** 和 **WBLT 平衡树**。
*Un-leafy Tree*：与 *Leafy Tree* 相反，将数据存放在每个树节点的数据结构。

## 替罪羊树

替罪羊树是最简单的平衡树，也是 *Un-leafy* 的，其核心思想是定义一个常数 $\alpha$，对于树上任意节点 $u$，若 $\max(siz_{ls_{u}}, siz_{rs_{u}}) \ge \alpha \times siz_u$，则将整棵树拍平成数组，对整棵树重构，使其保持平衡性。

替罪羊树的复杂度是 $O(n \log n)$ （**均摊**）的，同时 $\alpha$ 的取值对复杂度的影响是直接的，当 $\alpha$ 在区间 $[0.5,1]$ 之间时，复杂度时最优的，一般取 $\alpha = 0.7$。

## Treap

### 旋转 Treap

旋转 Treap 主要通过旋转来调整树的平衡。我们为每一个节点新加入一个键值 $priority$。Treap 除了在维护平衡树的性质外，还要维护对于 $priority$ 的堆的性质。而这一部分的维护靠的就是旋转操作。

复杂度：$O(n \log n)$ （期望）

### 无旋 Treap （FHQ-Treap）

无旋 Treap （又称 FHQ-Treap）通过分裂和合并的方式来维护树的平衡性。每次按照一个阈值将树分为两个部分，然后对树进行操作后，将两个部分合并。由于其基本操作为分裂和合并，所以可以较为简单的处理区间问题，而且码量较小，但常数较大。

复杂度：$O(n \log n)$ （期望）

## Splay

Splay 的基本操作为旋转 （双旋）每次将操作的节点旋转至根节点，所以每操作一次，树的形态都会发生改变。复杂度为 $O(n \log n)$ （均摊）

## WBLT (Weight Balanced Leafy Tree)

WBLT 是一种 Leafy 的平衡树，将所有信息存储在叶子节点上。同替罪羊树，WBLT 也是依靠设立常数 $\alpha$ 来保证其平衡性，复杂度为 $O(n \log n)$ （严格）

## 可持久化平衡树

路径复制，肥节点。