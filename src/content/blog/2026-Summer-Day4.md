---
title: '2026 Summer Day4'
description: ''
date: '2026-07-20T12:32:58.041Z'
draft: false
showHeroImage: false
tags: ['平衡树']
categories: ['记录']
series: ['2026 Summe']
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day4

## 练习

### P4402 [CERC2007] robotic sort 机械排序

[题目链接](https://www.luogu.com.cn/problem/P4402)

这道题就是**文艺平衡树**的~~弱化版~~，直接同文艺平衡树打一个 $rev$ 的标记，然后下传即可。

对于最小值的部分，直接记录每个节点管辖的区间的最小值，然后向下递归寻找最小值的排名即可。这里的排名是相对于序列编号而言的。

[code](https://www.luogu.com.cn/record/287296992)

### P4847 银河英雄传说V2

[题目链接](https://www.luogu.com.cn/problem/P4847)

这道题也是 FHQ-Treap 的板子。对于操作 `M`, 每次暴力向上跳找到 $x$ 所在 Treap 的根，然后合并即可。其他的就是普通的 FHQ-Treap 的操作了。

[code](https://www.luogu.com.cn/record/287324124)

### P2042 [NOI2005] 维护数列

[题目链接](https://www.luogu.com.cn/problem/P2042)

这道题还是有一个小 trick 的，对于插入操作，由于是插入一整段，如果单独对每一个 split 和 merge 的话，会由于常数巨大像 ~~xzy 大佬一样爆掉~~。所以我们对于那一整段单独构建一棵 Treap 后，再和原来的 Treap 合并。这样就保证了常数不会过大而爆掉，复杂度 $O(n \log n)$。

然后还要注意，由于要求最大字段和，所以要维护前缀和后缀的信息，所以 **reverse 的时候要将前缀和后缀的信息也 swap 一下**。

```cpp
inline void rev(int u) {
  tr[u].rev = !tr[u].rev;
  swap(tr[u].lc, tr[u].rc);
  // WARN: 这里要把前后缀的值也修改，因为交换了左右子树
  swap(tr[u].pre, tr[u].suf);
}
```

[code](https://www.luogu.com.cn/record/287402068)
