---
title: '2025 Summer Day26'
description: '暑期集训 Day26'
date: '2025-08-07T03:22:39.534Z'
draft: false
tags:
  - 图论
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day26


# 课堂内容

## 差分约束

### Problem

给定一个包含 $n$ 个不等式的不等式组，要求求出这个不等式组的任意一组解，或者判断不等式组无解。

### Solution

对于给定的这个问题，整理一下，发现要求解：

$$
\begin{cases}
x_1 - x_{2} > c_{1} \\
x_{2} - x_{3} > c_{2} \\
x_{3} - x_{4} > c_{3} \\
\dots \\
x_{n} - x_{1} > c_{n}
\end{cases} \Rightarrow \begin{cases}
x_1 > x_{2} + c_{1} \\
x_{2} > x_{3} + c_{2} \\
x_{3} > x_{4} + c_{3} \\
\dots \\
x_{n} > x_{1} + c_{n}
\end{cases}
$$

容易发现这些不等式的形式和 *Dijkstra* 中三角不等式 $dis_{u} > dis_{v} + w$ 类似。更进一步地，我们发现如果将不等式组中的不等式转化成边 $(x_{i},x_{i+1},c_{i})$ 的话，其实的解就是 $dis_i$。如果原问题无解的话，相当于在图上存在负环。至此，问题解决。

需要注意的是，因为要判断是否存在负环，所以不能使用 **Dijkstra**，而是要使用 **SPFA** 解决此问题。

## LCA（最近公共祖先）

### Problem

给点一棵树，每次询问  $(u,v)$ 的最近公共祖先 $LCA(u,v)$。

### Solution

#### 倍增

首先我们处理出所有点的直接父亲是谁，然后利用倍增的思路，处理出这个点向上跳一条长度为 $2^k$ 的链后它的父亲是谁。

具体地，我们需要得到一个数组 $jump[u][k]$ 表示节点 $u$ 向上跳 $2^k$ 步后他的父亲是谁，即 $\displaystyle jump[u][k] = jump[jump[u][k - 1]][k-1]$，先向上跳 $2^{k-1}$ 步，再跳 $2^{k-1}$ 步，就得到了我们想要的答案。

查询的时候只需要暴力向上跳即可，复杂度 $O(m \log n)$。

#### 树剖

我们先对原树做重链剖分，然后查询的时候每次跳一条重链，直到两条重链的链头的父亲相同。复杂度 $O(m \log n)$。

#### ST 表

我们在原树上处理出欧拉序，由于原树上的任意一颗子树在欧拉序上对应了一个连续的区间，所以问题就转化为了查询这个区间内深度最小的节点的 *RMQ* 问题，可以用 *ST* 表解决，复杂度为预处理 $O(n \log n)$，查询 $O(1)$。

#### Tarjan（离线）

我们将所有询问离线下来，然后在 Tarjan 的时候判断与当前这个点有关的所有查询中，是否存在另一个点已经被访问了的查询，如果存在，就更新即可。期间用并查集维护 LCA，复杂度 $O(\alpha(m + n,n) + n)$。

# 例题

## [ARC084B](https://atcoder.jp/contests/math-and-algorithm/tasks/arc084_b?lang=en) Small Multiple

### Problem

给定一个数字 $k$，要求找到最小的 $k$ 的倍数，使得所有数位上的数字和最小。

### Solution

我们不难发现，一个数 $x \mid k$ 等价于如果我们用 $c_{i}$ 表示 $x$ 的每一个数位上的数，则 $\forall i,c_{i} \times 10^i \bmod k = 0$。所以我们可以对于每一个数位考虑。不难发现，每一次操作就是 $\times 10$ 或者 $+1$，所以我们可以 0/1 BFS。

提交记录：[Link](https://atcoder.jp/contests/math-and-algorithm/submissions/68406044)

## [Luogu-P1993](https://www.luogu.com.cn/problem/P1993) 小 K 的农场

### Problem

给定共 $m$ 个以下形式的不等式组：

1. $a-b \ge c$；
2. $a-b \le c$；
3. $a = b$；

求其中任意一组解。

### Solution

我们将题目中的不等式组变换一下：

1. $a-b \ge c \to b \le a + c$；
2. $a-b \le c \to a \le b + c$；
3. $a = b \to a \le b$ 和 $b \le a$；

所以我们按照差分约束的方式建图即可。

提交记录：[Link](https://www.luogu.com.cn/record/list?pid=P1993&user=1023191)

## [CodeForces-776D](https://www.codeforces.com/problemset/problem/776/D) The Door Problem

### Problem

给你 $n$ 扇门和 $m$ 个开关，每个开关控制 $k$ 扇不同的门，一扇门最多被两个开关控制。初始时每扇门要么打开，要么关闭。变换一个开关的状态会使门的状态也发生变化，问你存不存在一种方案，使得所有门均开启。

### Solution

不难发现，最初打开的门所对应的开关要么同时打开，要么同时关闭，所以我们可以用并查集维护每个开关的状态。若果这个门最初是关闭的，则将 $(x,y)$ 和 $(x+m,y+m)$ 合并，否则将 $(x+m,y)$ 和 $(y + m,x)$ 合并。最后检查每个开关的状态是否合法即可。

提交记录：[Link](https://vjudge.net/solution/62765202)

## 模拟题-5.2 第 K 大查询

### Problem

给定一个 $1 \backsim n$ 的排列 $a$，想知道所有满足 $r-l+1\ge k$ 的区间 $[l,r]$ 内第 K 大的数的和是多少。

### Solution

我们将原来的排列按照从小到大排序，发现一个数在区间 $[l,r]$ 是第 $k$ 大当且仅当区间中包含 $k-1$ 个比它大的数。所以我们可以用双向链表维护这个思路。每次暴力像两边扩展，然后计算贡献，最后在双向链表中删除这个数。时间复杂度 $O(nk)$。

### Code

```c
#include <algorithm>
#include <iostream>

using std::cin;
using std::cout;

constexpr int N = 5e5 + 5;
int n, k, arr[N], index[N];
long long answer = 0;

struct Node {
    int left, right;
} link[N];

int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    
    cin >> n >> k;
    
    for (int i = 1; i <= n; i++) {
        cin >> arr[i];
        index[i] = i;
        link[i] = (Node) {i - 1, i + 1};
    }
    
    std::sort(index + 1, index + n + 1, [&] (int a, int b) { return arr[a] < arr[b]; });
    
    for (int i = 1; i <= n; i++) {
        int _index = index[i];
        int left = _index, right = _index;
        int rank = 1;
        
        while (link[left].left != 0 && rank < k) {
            rank++;
            left = link[left].left;
        }
        
        while (link[right].right != n + 1 && rank < k) {
            rank++;
            right = link[right].right;
        }
        
        // cout << left << ' ' << right << "\n";
        
        if (rank == k) {
            while (left != link[_index].right && right != n + 1) {
                answer += 1ll * (left - link[left].left) * (link[right].right - right) * arr[_index];
                left = link[left].right;
                right = link[right].right;
            }
        }
        
        link[link[_index].left].right = link[_index].right;
        link[link[_index].right].left = link[_index].left;
    }
    
    cout << answer << '\n';
    
    return 0;
}
```