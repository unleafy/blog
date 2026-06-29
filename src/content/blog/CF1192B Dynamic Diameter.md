---
title: "CF1192B Dynamic Diameter"
description: "CF1192B Dynamic Diameter"
date: 2025-06-29T03:22:39.534Z
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


# CF1192B Dynamic Diameter


## 算法
+ 欧拉序
+ 线段树


## 思路
对于这道题考虑使用欧拉序的性质：
+ 对于树上的每一个子树，在欧拉序上都有一个区间与之对应。
+ 对于 $(u, v)$ 的最近公共祖先，在欧拉序上表现为区间 $[pos_u, pos_v]$ 内深度最浅的节点。

所以可以将原树转化为欧拉序，将树上问题转化为序列上的区间问题，就可以使用线段树解决。

由于题目要求树的直径，所以用线段树查找区间最值，找到点 $(u, v)$ 的最近公共祖先 $lca$，则 $dist(u, v)$ 可以表示为：

$$
\large
dist(u,v) = dep_u + dep_v - 2 \cdot dep_{lca}
$$

而直径就是 $\max\{dist(u, v)\}$。

所以原题目的问题就转化为：在欧拉序上找一个三元组 $(u, lca, v)$，满足如下条件：
+ $dep_u + dep_v - 2 \cdot dep_{lca}$ 最大。
+ $lca$ 为区间 $[pos_u, pos_v]$ 内深度最小的节点。

而第二个条件可以由第一个条件限制，所以可以忽略。

由上面两个限制条件我们可以得到线段树需要维护的数据：

+ $max$：区间最大值。
+ $min$：区间最小值。
+ $left\ mid$（以下简称 $LM$）：三元组 $(u, lca, v)$ 中 $dep_u - 2 \cdot dep_{lca}$ 的最大值。
+ $mid\ right$（以下简称 $RM$）：三元组 $(u, lca, v)$ 中 $dep_v - 2 \cdot dep_{lca}$ 的最大值。
+ $left\ mid\ right$（以下简称 $LMR$）：三元组 $(u, lca, v)$ 中 $dep_u + dep_v - 2 \cdot dep_{lca}$ 的最大值。

接下来考虑如何转移 （即为 **push_up(int k)** 函数）。
对于 $max$ 和 $min$ 来说转移很简单，即为：


$$
\large
\begin{aligned}
max_k &= \max(max_{lc}, max_{rc}) \\
min_k &= \min(min_{lc}, min_{rc}) \\
\end{aligned}
$$


而对于 $LM$ 而言，可以从三个方向转移：


$$
\large
LM = \max(LM_{lc}, LM_{rc}, max_{lc} - 2 \cdot min_{rc})
$$


对于 $MR$ 同理由：


$$
\large
MR = \max(MR_{lc}, MR_{rc}, -2 \cdot min_{lc} + max_{rc}) 
$$




最后合并成 $LMR$:


$$
\large
LMR = \max(LMR_{lc}, LMR_{rc}, LM_{lc} + max_{rc}, max_{lc} + MR_{rc})
$$



## 代码
```c
#include <bits/stdc++.h>
#define OnlineJudge

constexpr int N = 1e5 + 5;
std::vector<int> euler_seq{-1};
int in[N], out[N], num = 0;
int head[N], cnt = 0;
int n, q, u, v, fa[N];
long long W, d, w, last = 0ll, dist[N];
std::vector<std::tuple<int, int, long long>> input_edges;

class Edge {
 public:
  int to, next;
  long long w;

} edges[N << 4];

void add_edge(int u, int v, long long w) {
  edges[cnt].to = v;
  edges[cnt].w = w;
  edges[cnt].next = head[u];
  head[u] = cnt++;
}

class SegmentTree {
 private:
  class Values {
    public:
    // ! 对 lazy 要初始化（虽然在 build 里面写了，但不知道为什么有些还是没有初始化）
    long long max = 0, min = 0, left_mid = 0, mid_right = 0, left_mid_right = 0, lazy = 0;
  };
 
  class NodeInfo {
   public:
    int left = 0, right = 0;
    Values val;
  };
  
  public:
  NodeInfo tr[N << 2];
  
  Values merge_tags(Values a, Values b) {
    Values retval;
    
    retval.max = std::max(a.max, b.max);
    retval.min = std::min(a.min, b.min);
    retval.left_mid = std::max({a.left_mid, b.left_mid, a.max + -2 * b.min});
    retval.mid_right = std::max({a.mid_right, b.mid_right, -2 * a.min + b.max});
    retval.left_mid_right = std::max({a.left_mid_right, b.left_mid_right, a.left_mid + b.max, a.max + b.mid_right});
    
    return retval;
  }
  
  void modify_tags(Values& root, long long value) {
    root.lazy += value;
    root.max += value;
    root.min += value;
    root.left_mid -= value;
    root.mid_right -= value;
  }
  
  void push_up(int k) {
    Values lc = tr[k << 1].val, rc = tr[k << 1 | 1].val;
    
    int temp = tr[k].val.lazy;
    tr[k].val = merge_tags(lc, rc);
    tr[k].val.lazy = temp;
  }
  
  void push_down(int k) {
    if (tr[k].val.lazy == 0) {
      return void();
    }
    
    modify_tags(tr[k << 1].val, tr[k].val.lazy);
    modify_tags(tr[k << 1 | 1].val, tr[k].val.lazy);
    
    tr[k].val.lazy = 0;
  }
  
  void build_tree(int k, int l, int r) {
    tr[k].left = l;
    tr[k].right = r;
    tr[k].val.lazy = 0;
    
    if (tr[k].left == tr[k].right) {
      tr[k].val.max = tr[k].val.min = dist[euler_seq[l]];
      tr[k].val.left_mid = tr[k].val.mid_right = -dist[euler_seq[l]];
      tr[k].val.left_mid_right = 0;
      
      #ifndef OnlineJudge
      std::cout << "BUILD:\n";
      std::cout << k << " " << tr[k].left << " " << tr[k].right << " ";
      std::cout << tr[k].val.max << " " << tr[k].val.min << " " << tr[k].val.left_mid << " ";
      std::cout << tr[k].val.mid_right << ' ' << tr[k].val.left_mid_right << " " << tr[k].val.lazy << '\n';
      #endif
      
      return void();
    }
    
    int mid = (tr[k].left + tr[k].right) >> 1;
    int lc = k << 1, rc = k << 1 | 1;
    
    build_tree(lc, l, mid);
    build_tree(rc, mid + 1, r);
    
    push_up(k);
    
    #ifndef OnlineJudge
    std::cout << "BUILD:\n";
    std::cout << k << " " << tr[k].left << " " << tr[k].right << " ";
    std::cout << tr[k].val.max << " " << tr[k].val.min << " " << tr[k].val.left_mid << " ";
    std::cout << tr[k].val.mid_right << ' ' << tr[k].val.left_mid_right << " " << tr[k].val.lazy << '\n';
    #endif
  }
  
  void modify(int k, int l, int r, long long value) {
    #ifndef OnlineJudge
    std::cout << "MODIFY:\n";
    std::cout << k << " " << tr[k].left << " " << tr[k].right << " ";
    std::cout << tr[k].val.max << " " << tr[k].val.min << " " << tr[k].val.left_mid << " ";
    std::cout << tr[k].val.mid_right << ' ' << tr[k].val.left_mid_right << " " << tr[k].val.lazy << '\n';
    #endif
    
    if (tr[k].left >= l && tr[k].right <= r) {
      modify_tags(tr[k].val, value);
      return void();
    }
    
    push_down(k);
    
    int mid = (tr[k].left + tr[k].right) >> 1;
    int lc = k << 1, rc = k << 1 | 1;
    
    if (r <= mid) {
      modify(lc, l, r, value);
    } else if (l > mid) {
      modify(rc, l, r, value);
    } else {
      modify(lc, l, mid, value);
      modify(rc, mid + 1, r, value);
    }
    
    push_up(k);
  }
  
  Values query(int k, int l, int r) {
    #ifndef OnlineJudge
    std::cout << "QUERY:\n";
    std::cout << k << " " << tr[k].left << " " << tr[k].right << " ";
    std::cout << tr[k].val.max << " " << tr[k].val.min << " " << tr[k].val.left_mid << " ";
    std::cout << tr[k].val.mid_right << ' ' << tr[k].val.left_mid_right << " " << tr[k].val.lazy << '\n';
    #endif
    
    if (tr[k].left >= l && tr[k].right <= r) {
      return tr[k].val;
    }
    
    push_down(k);
    
    int mid = (tr[k].left + tr[k].right) >> 1;
    int lc = k << 1, rc = k << 1 | 1;
    
    if (r <= mid) {
      return query(lc, l, r);
    } else if (l > mid) {
      return query(rc, l, r);
    } else {
      return merge_tags(query(lc, l, mid), query(rc, mid + 1, r));
    }
  }
} data;

void dfs(int u, int father) {
  euler_seq.emplace_back(u);
  in[u] = euler_seq.size() - 1;
  
  for (int i = head[u]; ~i; i = edges[i].next) {
    int v = edges[i].to;
    if (v == father) continue;

    fa[v] = u;
    dist[v] = dist[u] + edges[i].w;
    dfs(v, u);
    euler_seq.emplace_back(u);
  }
  
  out[u] = euler_seq.size() - 1;
}

int main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(nullptr);
  std::cout.tie(nullptr);
  
  std::memset(head, -1, sizeof(head));
  
  std::cin >> n >> q >> W;
  
  for (int i = 1; i < n; i ++) {
    std::cin >> u >> v >> w;
    
    add_edge(u, v, w);
    add_edge(v, u, w);
    input_edges.emplace_back(u, v, w);
  }
  
  dfs(1, 1);
  
  // ! 注意调换顺序，保证前面一个点是后面一个点的父亲
  for (int i = 0; i < n - 1; i ++) {
    if (fa[std::get<0>(input_edges[i])] == std::get<1>(input_edges[i])) {
      std::swap(std::get<0>(input_edges[i]), std::get<1>(input_edges[i]));
    }
  }
  
  data.build_tree(1, 1, euler_seq.size() - 1);
  
  #ifndef OnlineJudge
  std::cout << data.query(1, 1, euler_seq.size() - 1).left_mid_right << '\n';
  for (auto&& [u, v, w] : input_edges) {
    std::cout << u << ' ' << v << ' ' << w << '\n';
  }
  #endif
  
  last = 0;
  for (int i = 1; i <= q; i ++) {
    std::cin >> d >> w;
    d = (d + last) % (n - 1);
    w = (w + last) % W;
    
    // ! 对于边 (u, v, w)，满足 fa[v] == u，则修改后的 w' 只对 v 的子树有影响，所以只更新 v 的子树即可。
    data.modify(1, in[std::get<1>(input_edges[d])], out[std::get<1>(input_edges[d])], w - std::get<2>(input_edges[d]));
    // ! 记得将边的信息更新
    std::get<2>(input_edges[d]) = w;
    last = data.query(1, 1, euler_seq.size() - 1).left_mid_right;
    
    std::cout << last << '\n';
  }
  
  return 0;
}
```