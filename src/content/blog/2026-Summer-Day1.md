---
title: '2026 Summer Day1'
description: ''
date: 2026-07-17:20:48.000Z
draft: false
showHeroImage: false
tags: ['数据结构', '并查集', '线段树']
categories: ['记录']
series: ['2026 Summer']
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day1

> Delay always breeds danger, and to protract a great design is often to ruin it.
> 拖延总是滋生危险，而延长一项伟大的计划往往会导致它的毁灭。

## 课堂内容

### 并查集

其实主要还是去年讲过的一些东西，但是也有一些新的思路。

比如上课有讲到，如果我们需要维护点对之间的带权联通的话，可以考虑原来就有的边 $(u, x, w_1)$ 和 $(v, y, w_2)$，这个时候如果我们需要联通 $(u, v, w_3)$ 的话，可以在 $x$ 和 $y$ 之间链接一条虚拟的边 $(x, y, w_1 + w_2 - w_3)$。因为你考虑 $u$ 和 $v$ 在同一个连通块中，他们之间的两两距离一定是相等的，否则就矛盾了，所以在某种特殊关系的并查集连边中，可以使用带权并查集维护连通性。

### 线段树和树状数组

这一部分也没有什么太大的变化，和去年差不多，但是群友总结的也很好：

> 树状数组短小，线段树广泛。

所以适当选择线段树和树状数组即可。

然后还有一个 Trick 就是，对于一些连续段的问题，可以用线段树维护每个区间的前缀和后缀，然后合并即可，比较 ~~基础~~（实则自己忘记了）

## 练习

### ABC380 E 1D Bucket Tool

[link](https://atcoder.jp/contests/abc380/tasks/abc380_e)

这道题就是维护相同的颜色连续段，因为你会发现如果相邻的两个点 merge 了，那么他们在后面的操作中是等价的，所以可以用并查集维护，然后并查集还要维护当前连续段的左右端点，每次修改的时候判断颜色是否相同 merge 即可。

[code](https://vjudge.net/solution/71073863)

### P6492 [COCI 2010/2011 #6] STEP

[link](https://luogu.com.cn/problem/P6492)

这道题就是回顾一下上面的那个 Trick，维护一下每个区间左右端点的字符，然后维护前缀后缀满足条件的最大长度，向上合并即可。

[code](https://www.luogu.com.cn/record/286738841)

### P1084 [NOIP 2012 提高组] 疫情控制

[link](https://www.luogu.com.cn/problem/P1084)

不难发现，每一个军队向上挑可以覆盖的路径数量是最多的，收益也是最大的，所以每一个军队只会在规定时间内不断向上跳，知道不合法为止。这一部分二分时间，倍增 check 即可。

但是 check 也有一些细节问题，如果一个军队可以跳到根节点，而且还有时间剩余的话，他可以走过根节点去覆盖那些还违背覆盖的根节点的儿子，双指针处理。

[code](https://www.luogu.com.cn/record/286759134)

## 后记

刚刚住进宿舍，感觉还不是很适应，没怎么睡好，下午好困。
