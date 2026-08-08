---
title: '2026 Summer Day20'
description: ''
date: '2026-08-06T12:03:01.788Z'
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

# 2026 Summer Day20

## CF1200 E - Compress Words

[题目链接](https://codeforces.com/problemset/problem/1200/E)

每次枚举可以匹配的长度 $j$，然后使用 _hash_ 判断两个字符串切片是否相等。复杂度 $O(\sum |S|)$。

[code](https://codeforces.com/contest/1200/submission/385820289)

## P3823 [NOI2017] 蚯蚓排队

[题目链接](https://www.luogu.com.cn/problem/P3823)

考虑暴力枚举修改的位置的前后 $50$ 个位置的状态，然后使用手写 _hash_ 表暴力合并信息，单次修改复杂度 $O(k^2)$，可以使用链表维护前后位置关系。查询的时候遍历 $s$ 的每一个长度为 $k$ 的子串，单次查询复杂度 $O(|S|)$。

蒟蒻在这道题中第一次手写 _hash_ 表，直接被肘飞了，还以为自己写错了，后来才发现是自己的模数太烂了，有没有神秘模数技巧。

[code](https://www.luogu.com.cn/record/290992397)
