---
title: '2025 Summer Day2'
description: '暑期集训 Day2'
date: '2025-07-16T03:22:39.534Z'
draft: false
tags: 
  - 数据结构
  - 线段树
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day2


**Content**：Segment Tree

**Date：2025.7.18**


## 主题

+ 线段树进阶

## 关于线段树

### 区间操作

+ 对于区间开根号我们可以记录最大值和最小值，然后维护**极差**，由此将区间开根号转化为区间加和区间覆盖问题，减小修改操作的复杂(度，均摊后复杂度为 $\Theta(n \log n \log^2 V)$。（题目：[HDU 5828](https://www.acm.hdu.edu.cn/showproblem.php?pid=5828)）
+ 对于区间取模的操作，我们依然记录**最大值**，对于 $max < P$ 的区间不做修改，从而降低复杂度。（题目：[CodeForces 438D](https://www.codeforces.com/contest/438/problem/D)）
+ 对于区间 $gcd$ 操作，我们可以对原数组进行**差分**，得到差分数组 $d$，而区间 $[l, r]$ 的 $gcd$ 即为 $gcd(a_l, d_{l + 1}, d_{l + 2}, \dots, d_{r})$。（题目：[洛谷 P10463](https://www.luogu.com.cn/problem/P10463)）


### 二维数点问题

+ 二维数点问题是在一个平面中，有若干个点 $(x_i, y_i)$，询问你在矩形 $(a, b)$，$(c, d)$ 中包括了多少个点。
+ 离线后的二维数点问题可以用线段树扫描线解决，相当于在线的**主席树**。
+ 题目：[CodeForces 1221F](https://www.codeforces.com/contest/1221/problem/F)。




```c
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6 + 5, M = 1e6 + 5;
int n;
vector<int> pos;
vector<pair<int, int>> a[M];

class Point {
 public:
  int x, y, w;
} points[N];

class SegmentTree {
  class Values {
   public:
    long long max, add;
    int index;
  };
  
  class Node {
   public:
    int left, right;
    Values val;
    
    Node() = default;
    Node(int l, int r) : left(l), right(r), val() {}
  };
  
  Node tr[M << 2];
  
  static int get_lc(int k) { return k << 1; }
  static int get_rc(int k) { return k << 1 | 1; }
  int get_mid(int k) { return (tr[k].left + tr[k].right) >> 1; }
  
  void make_lazy_add(int k, long long val) {
    tr[k].val.add += val;
    tr[k].val.max += val;
  }
  
  void push_up(int k) {
    int lc = get_lc(k), rc = get_rc(k);
    
    if (tr[lc].val.max > tr[rc].val.max) {
      tr[k].val.max = tr[lc].val.max;
      tr[k].val.index = tr[lc].val.index;
    } else {
      tr[k].val.max = tr[rc].val.max;
      tr[k].val.index = tr[rc].val.index;
    }
  }
  
  void push_down(int k) {
    if (tr[k].val.add == 0)
      return void();
    
    int lc = get_lc(k), rc = get_rc(k);
    
    make_lazy_add(lc, tr[k].val.add);
    make_lazy_add(rc, tr[k].val.add);
    
    tr[k].val.add = 0;
  }
  
 public:
  void build_tree(int k, int l, int r) {
    tr[k] = Node(l, r);
    
    if (tr[k].left == tr[k].right) {
      tr[k].val.max = -pos[l - 1];
      tr[k].val.index = l;
      return void();
    }
    
    int mid = get_mid(k);
    int lc = get_lc(k), rc = get_rc(k);
    
    build_tree(lc, l, mid);
    build_tree(rc, mid + 1, r);
    
    push_up(k);
  }
  
  void modify(int k, int l, int r, int val) {
    if (tr[k].left >= l && tr[k].right <= r) {
      make_lazy_add(k, val);
      return void();
    }
    
    push_down(k);
    
    int mid = get_mid(k);
    int lc = get_lc(k), rc = get_rc(k);
    
    if (r <= mid) {
      modify(lc, l, r, val);
    } else if (l > mid) {
      modify(rc, l, r, val);
    } else {
      modify(lc, l, mid, val);
      modify(rc, mid + 1, r, val);
    }
    
    push_up(k);
  }
  
  pair<long long, long long> query(int k, int l, int r) {
    if (tr[k].left >= l && tr[k].right <= r) {
      return make_pair(tr[k].val.max, tr[k].val.index);
    }
    
    push_down(k);
    
    int mid = get_mid(k);
    int lc = get_lc(k), rc = get_rc(k);
    
    if (r <= mid) {
      return query(lc, l, r);
    } else if (l > mid) {
      return query(rc, l, r);
    } else {
      return max(query(lc, l, mid), query(rc, mid + 1, r));
    }
  }
} seg;

int get_index(int val) {
  return lower_bound(pos.begin(), pos.end(), val) - pos.begin() + 1;
}

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  
  cin >> n;
  
  for (int i = 1; i <= n; i ++) {
    cin >> points[i].x >> points[i].y >> points[i].w;
    
    pos.emplace_back(points[i].x);
    pos.emplace_back(points[i].y);
    
    if (points[i].x > points[i].y) {
      swap(points[i].x, points[i].y);
    }
  }
  
  sort(pos.begin(), pos.end());
  pos.erase(unique(pos.begin(), pos.end()), pos.end());
  
  int limit = 0;
  for (int i = 1; i <= n; i ++) {
    points[i].x = get_index(points[i].x);
    points[i].y = get_index(points[i].y);
    
    limit = max(limit, points[i].y);
    
    a[points[i].x].emplace_back(points[i].y, points[i].w);
  }
  
  seg.build_tree(1, 1, limit);
  
  long long answer = -1ll << 60, left = 0, right = 0;
  for (int line = limit; line >= 1; line --) {
    for (auto p : a[line]) {
      seg.modify(1, p.first, limit, p.second);
    }
    
    pair<long long, long long> result = seg.query(1, line, limit);
    result.first += pos[line - 1];
    
    if (result.first > answer) {
      answer = result.first;
      left = pos[line - 1];
      right = pos[result.second - 1];
    }
  }
  
  if (answer < 0) {
    answer = 0;
    left = 1e9 + 1, right = 1e9 + 1;
  }
  
  cout << answer << "\n";
  cout << left << " " << left << " " << right << " " << right << '\n';
  
  return 0;
}
```

### 线段树分治

+ 线段树分治问题，即在线段树上进行递归，依据线段树的结构性质完成的一类问题。
+ 这类问题的特点是：具有明显的前后/时间关系，且删除操作不易实现，但插入操作可以较容易的实现。
+ 题目：[洛谷 P5787](https://www..luogu.com.cn/problem/P5787)


```c
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 5;
bool satisfied[N];
int n, m, k, u, v, l, r;

class DSU {
  int fa[N << 1], size[N << 1];
  stack<pair<int *, int>> stk;

 public:
  void init(int limit) {
    for (int i = 1; i <= limit; i++) fa[i] = i, size[i] = 1;
  }

  int find(int x) {
    if (fa[x] != x) return find(fa[x]);
    return x;
  }

  void merge(int u, int v) {
    u = find(u);
    v = find(v);
    if (u == v) return void();

    if (size[u] > size[v]) swap(u, v);

    stk.emplace(fa + u, fa[u]);
    stk.emplace(size + v, size[v]);
    fa[u] = v;
    size[v] += size[u];
  }

  bool check(int x, int y) { return find(x) == find(y); }

  int get_version() { return stk.size(); }

  void undo(int version) {
    while (stk.size() > version) {
      *stk.top().first = stk.top().second;
      stk.pop();
    }
  }
} d;

class SegmentTree {
  class Node {
   public:
    int left, right;
    vector<pair<int, int>> edges;

    Node() = default;
    Node(int l, int r) : left(l), right(r) { edges.clear(); }
  };

  Node tr[N << 2];

  static int get_lc(int k) { return k << 1; }
  static int get_rc(int k) { return k << 1 | 1; }
  int get_mid(int k) { return (tr[k].left + tr[k].right) >> 1; }

 public:
  void build_tree(int k, int l, int r) {
    tr[k] = Node(l, r);

    if (tr[k].left == tr[k].right) {
      return void();
    }

    int mid = get_mid(k);
    int lc = get_lc(k), rc = get_rc(k);

    build_tree(lc, l, mid);
    build_tree(rc, mid + 1, r);
  }

  void add_edge(int k, int l, int r, pair<int, int> edge) {
    if (tr[k].left >= l && tr[k].right <= r) {
      tr[k].edges.emplace_back(edge);
      return void();
    }

    int mid = get_mid(k);
    int lc = get_lc(k), rc = get_rc(k);

    if (r <= mid) {
      add_edge(lc, l, r, edge);
    } else if (l > mid) {
      add_edge(rc, l, r, edge);
    } else {
      add_edge(lc, l, mid, edge);
      add_edge(rc, mid + 1, r, edge);
    }
  }

  void dfs(int k) {
    bool flag = true;
    int version = d.get_version();

    for (auto edge : tr[k].edges) {
      if (d.check(edge.first, edge.second)) {
        flag = false;
        break;
      } else {
        d.merge(edge.first, edge.second + n);
        d.merge(edge.first + n, edge.second);
      }
    }

    if (flag == false) {
      for (int i = 1; i <= tr[k].right - tr[k].left + 1; i++) {
        cout << "No\n";
      }
    } else {
      if (tr[k].left == tr[k].right) {
        cout << "Yes\n";
      } else {
        int lc = get_lc(k), rc = get_rc(k);

        dfs(lc);
        dfs(rc);
      }
    }

    d.undo(version);
  }
} seg;

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);

  cin >> n >> m >> k;

  d.init(n << 1);
  seg.build_tree(1, 1, k);

  for (int i = 1; i <= m; i++) {
    cin >> u >> v >> l >> r;

    if (l == r) continue;
    seg.add_edge(1, l + 1, r, make_pair(u, v));
  }

  seg.dfs(1);

  return 0;
}
```