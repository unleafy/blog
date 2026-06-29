---
title: "SP1716 GSS3 - Can you answer these queries III"
description: "SP1716 GSS3 - Can you answer these queries III"
date: 2025-7-5T03:22:39.534Z
draft: false
tags:
  - 线段树
series:
  - '题解'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---


# SP1716 GSS3 - Can you answer these queries III


## 算法

+ 线段树




## 思路

题目要求维护支持单点修改和查询区间最大子段和的数据结构。
考虑使用线段树维护。对于区间 $[l_1,r_1], [l_2,r_2]$ ，合并后的最大子段和有一下三种情况：

1. 区间 $[l_1,r_1]$ 的最大子段和。
2. 区间 $[l_2,r_2]$ 的最大子段和。
3. 跨过中间，区间 $[l_1,r_1]$ 的右边部分和区间 $[l_2,r_2]$ 的左边部分合并后的最大子段和。

所以线段树要维护的信息有四个：
1. 区间和
2. 区间最大子段和
3. 区间左侧最大子段和。
4. 区间右侧最大子段和。

对以上四个信息分别维护即可。


## 代码


```c
#include <bits/stdc++.h>

constexpr int N = 5e4 + 5;
int n, m, op, l, r, arr[N];

class SegmentTree {
 protected:
  class Values {
   public:
    int sum = 0, max = 0, left_max = 0, right_max = 0;

    Values() = default;

    Values(int x) {
      sum = x;
      max = x;
      left_max = x;
      right_max = x;
    }
  };

  class Node {
   public:
    int l, r;
    Values val;
  };

  Node tr[N << 2];

 public:
  Values merge(Values a, Values b) {
    Values retval;

    retval.sum = a.sum + b.sum;
    retval.max = std::max({a.max, b.max, a.right_max + b.left_max});
    retval.left_max = std::max(a.left_max, a.sum + b.left_max);
    retval.right_max = std::max(b.right_max, b.sum + a.right_max);

    return retval;
  }

  void push_up(int k) {
    Values lc = tr[k << 1].val, rc = tr[k << 1 | 1].val;
    tr[k].val = merge(lc, rc);
  }

  void build_tree(int k, int l, int r) {
    tr[k].l = l, tr[k].r = r;
    tr[k].val = Values(0);

    if (tr[k].l == tr[k].r) {
      tr[k].val = Values(arr[l]);
      return void();
    }

    int mid = (tr[k].l + tr[k].r) >> 1;
    int lc = k << 1, rc = k << 1 | 1;

    build_tree(lc, l, mid);
    build_tree(rc, mid + 1, r);

    push_up(k);
  }

  void modify(int k, int pos, int value) {
    if (tr[k].l == tr[k].r && tr[k].r == pos) {
      tr[k].val = Values(value);
      return void();
    }

    int mid = (tr[k].l + tr[k].r) >> 1;
    int lc = k << 1, rc = k << 1 | 1;

    if (pos <= mid) {
      modify(lc, pos, value);
    } else {
      modify(rc, pos, value);
    }

    push_up(k);
  }

  Values query(int k, int l, int r) {
    if (tr[k].l >= l && tr[k].r <= r) {
      return tr[k].val;
    }

    int mid = (tr[k].l + tr[k].r) >> 1;
    int lc = k << 1, rc = k << 1 | 1;

    if (r <= mid) {
      return query(lc, l, r);
    } else if (l > mid) {
      return query(rc, l, r);
    } else {
      return merge(query(lc, l, mid), query(rc, mid + 1, r));
    }
  }
} segment_tree;

int main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(nullptr);
  std::cout.tie(nullptr);

  std::cin >> n;

  for (int i = 1; i <= n; i++) {
    std::cin >> arr[i];
  }

  segment_tree.build_tree(1, 1, n);
  
  std::cin >> m;

  while (m--) {
    std::cin >> op >> l >> r;
    
    if (op == 0) {
      segment_tree.modify(1, l, r);
    } else if (op == 1) {
      std::cout << segment_tree.query(1, l, r).max << '\n';
    }
  }

  return 0;
}
```