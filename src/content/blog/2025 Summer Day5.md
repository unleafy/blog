---
title: '2025 Summer Day5'
description: '暑期集训 Day5'
date: '2025-07-19T03:22:39.534Z'
draft: false
tags:
  - 树上问题
  - 点分治
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---


# 2025 Summer Day5


**Content**: Problem on Tree

**Date**：2025.7.21


# 概览

+ 树的重心
+ 树上启发式合并
+ 树链剖分
+ 左偏树
+ 点分治

# 具体内容

## 树的重心

### 定义

树的重心是满足如下条件的点 $u$:
+ 树上不存在其他节点 $v$，使得 $\max\{\operatorname{siz}(son_v)\} < \max\{\operatorname{size}(son_u)\}$。


### 性质

树的重心 $G$ 有如下性质：

+ 当以 $G$ 为根节点时，不能存在 $v$ 满足 $\max_{\forall v \in \operatorname{son}(u)}\{\operatorname{size}(v)\} > \frac{N}{2}$ ($N$为整棵树的大小)。反之亦然。
+ 树中所有节点到 $G$ 的**距离之和最小**，如果有两个重心，则距离**相同**。
+ 如果两棵树的重心分别为 $G_1$，$G_2$，则两棵树由一条边拼接起来之后，新树的重心**一定在 $G_1$ 到 $G_2$ 的路径上**。
+ 在一棵树上加入 (或删除) 一个叶子节点，其重心**只移动一条边的距离**。
+ 树的重心一定在**以根节点为链头的重链上**。


### 例题

[CF685B](www.vjudge.net/problem/CodeForces-685B) Kay and Snowflake


**题目大意**：

> 求一颗大小为 $n$ ($1 \le n \le 10^{5}$) 有根树的所有子树的重心。


考虑运用性质 $4$，对整棵树进行 $dfs$，每次用子节点的重心暴力向上跳，再用性质 $1$ 检查，复杂度 $\Theta(n \log n)$。

**Code**

```c
#include <bits/stdc++.h>

using namespace std;

constexpr int N = 3e5 + 5;
int head[N], cnt = 0;
int n, q, father[N], query_node;
int son[N], sub_size[N], answer[N];

struct Edge {
	int to, next;
} e[N << 1];

void add_edge(const int u, const int v) {
	e[cnt].to = v;
	e[cnt].next = head[u];
	head[u] = cnt++;
}

void dfs(const int u) {
	sub_size[u] = 1;

	for (int i = head[u]; ~i; i = e[i].next) {
		const int v = e[i].to;
		if (v == father[u]) continue;

		dfs(v);
		sub_size[u] += sub_size[v];

		if (son[u] == 0 || sub_size[v] > sub_size[son[u]]) {
			son[u] = v;
		}
	}

	if (son[u] == 0) {
		answer[u] = u;
	} else {
		answer[u] = answer[son[u]];

		while (answer[u] != u) {
			if (sub_size[son[answer[u]]] * 2 <= sub_size[u] && (sub_size[u] - sub_size[answer[u]]) * 2 <= sub_size[u]) {
				break;
			}
			answer[u] = father[answer[u]];
		}
	}
}

int main() {
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	memset(head, -1, sizeof(head));

	cin >> n >> q;

	for (int i = 2; i <= n; i++) {
		cin >> father[i];
		add_edge(i, father[i]);
		add_edge(father[i], i);
	}

	dfs(1);

	while (q--) {
		cin >> query_node;
		cout << answer[query_node] << '\n';
	}

	return 0;
}
```

## 树上启发式合并

### 算法内容

对于某些问题，暴力的复杂度远远大于 $std$ 的复杂度，我们可以考虑**启发式合并的思想**。
启发式合并时人类对于算法的主观优化，我们以按秩合并的并查集为例。

**并查集**

```c
struct DSU {
    int father[N], size[N];
    void init(int limit) {
        std::iota(father + 1, father + n + 1, 1);
        std::memset(size, 1, sizeof(size));
    }

    int find(int x) {
        if (father[x] != x) father[x] = find(father[x]);
        return father[x];
    }

    bool check(int x, int y) {
        return find(x) == find(y);
    }

    void merge(int x, int y) {
        x = find(x), y = find(y);
        if (x == y) return void();

        // 按秩合并
        if (size[x] > size[y]) {
            swap(x, y);
        }

        father[x] = y;
    }
}
```

通过人的主观感受，我们可以知道将大小更小的子树合并到大小更大的子树上一定是更优的。树上启发式合并也是这种思想。我们通过和树链剖分类似的方法，将子节点分为重儿子和轻儿子，对于统计答案时，我们对重儿子统计的答案保留，对轻儿子统计的答案进行撤销，这样保证了复杂度的正确性。

### 例题

[CF600E](www.vjudge.net/problem/CodeForces-600E) Lomsat gelral

**题目描述**
给定一个有根树，每个点有一个颜色 $c_i$，对每个子树，求所有出现最多的颜色的和。
$n \le 10^{5}$


我们开一个桶记录每一种颜色再子树内的出现次数，并同时维护两个变量：

+ $max$：当前出现最多的颜色出现的次数。
+ $sum$：出现最多的颜色的和。

每次我们遍历的时候先遍历轻儿子，再遍历重儿子，传入一个标记 $f$，表示当前的统计数据是否保留，并根据题意维护 $max$ 和 $sum$ 即可。

***Code***

```c
#include <bits/stdc++.h>

using std::cin;
using std::cout;

constexpr int N = 1e5 + 5;
int head[N], cnt = 0;
int n, col[N], u, v;
int tot[N], size[N], son[N], father[N], dfn = 0;
long long max = 0, sum = 0, answer[N];

class Edge {
public:
    int to, next;

    Edge(const int to = 0, const int next = 0) : to(to), next(next) {}
} e[N << 1];

void add_edge(const int u, const int v) {
    e[cnt] = Edge(v, head[u]);
    head[u] = cnt++;
}

void dfs1(const int u, const int fa) {
    size[u] = 1;
    father[u] = fa;
    
    for (int i = head[u]; ~i; i = e[i].next) {
        int v = e[i].to;
        if (v == fa) continue;

        dfs1(v, u);
        size[u] += size[v];

        if (son[u] == 0 || size[v] > size[son[u]]) {
            son[u] = v;
        }
    }
}

void update(const int u, const int val, const int ban) {
    tot[col[u]] += val;
    if (tot[col[u]] == max) {
        sum += col[u];
    } else if (tot[col[u]] > max) {
        max = tot[col[u]];
        sum = col[u];
    }
    
    for (int i = head[u]; ~i; i = e[i].next) {
        const int v = e[i].to;
        if (v == father[u] || v == ban) {
            continue;
        }
        
        update(v, val, ban);
    }
}

void dfs2(const int u, const bool heavy_son) {
    for (int i = head[u]; ~i; i = e[i].next) {
        const int v = e[i].to;
        if (v == father[u] || v == son[u]) {
            continue;
        }

        dfs2(v, false);
    }
    
    if (son[u] != 0) {
        dfs2(son[u], true);
    }
    
    update(u, 1, son[u]);
    
    answer[u] = sum;
    
    if (heavy_son == false) {
        update(u, -1, -1);
        sum = 0;
        max = 0;
    }
}

int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    
    std::memset(head, -1, sizeof(head));

    cin >> n;

    for (int i = 1; i <= n; i++) {
        cin >> col[i];
    }

    for (int i = 1; i < n; i++) {
        cin >> u >> v;
        add_edge(u, v);
        add_edge(v, u);
    }
    
    dfs1(1, 0);
    dfs2(1, true);
    
    for (int i = 1; i <= n; i++) {
        cout << answer[i] << " ";
    }

    return 0;
}
```

## 树链剖分

### 算法步骤

我们维护 $5$ 个数组：

+ $fa[u]$：表示节点 $u$ 的父亲。
+ $size[u]$：表示节点 $u$ 的子树大小。
+ $son[u]$：表示节点 $u$ 的重儿子 (即节点 $u$ 的子节点中子树大小最大的节点) 的编号。
+ $top[u]$：表示节点 $u$ 所在链的链头编号。
+ $id[i]$：表示 $dfn$ 值为 $i$ 的节点编号。

这样我们就把整棵树拆成了若干条链和轻边。这样我们就可以用线段树对整棵树进行维护 (每一条链都对应了一段连续的 $dfn$ 区间)。

### 例题

[洛谷 P3384](www.luogu.com.cn/problem/P3384) 重链剖分/树链剖分

根据题意对树剖完了的 $dfn$ 序列用线段树维护即可。

**Code**

```c
#include <bits/stdc++.h>
#define OnlineJudge

using std::cin;
using std::cout;

constexpr int N = 1e5 + 5;
int head[N], cnt = 0;
int n, m, root, P, u, v, initial_value[N], value[N], opt;
int top[N], size[N], son[N], dep[N], fa[N], id[N], num = 0;

class Edge {
public:
    int to, next;

    Edge(const int to = 0, const int next = 0) : to(to), next(next) {}
} e[N << 1];

void add_edge(int u, int v) {
    e[cnt] = Edge(v, head[u]);
    head[u] = cnt++;
}

struct SegmentTree {
    struct Node {
        int left, right;
        long long sum, add;

        explicit Node(const int l = 0, const int r = 0, const int s = 0, const int a = 0) :
            left(l), right(r), sum(s), add(a) {}
    };

    Node tr[N << 2];

    SegmentTree() {}

    void make_lazy(const int k, const long long value) {
        (tr[k].sum += (tr[k].right - tr[k].left + 1) * value) %= P;
        (tr[k].add += value) %= P;
    }

    void push_up(const int k) {
        const int left_child = k << 1, right_child = k << 1 | 1;
        tr[k].sum = tr[left_child].sum + tr[right_child].sum;
        tr[k].sum %= P;
    }

    void push_down(const int k) {
        if (tr[k].add == 0) return void();

        const int left_child = k << 1, right_child = k << 1 | 1;
        make_lazy(left_child, tr[k].add);
        make_lazy(right_child, tr[k].add);

        tr[k].add = 0;
    }

    void build_tree(const int k, const int left, const int right) {
        tr[k] = Node(left, right);

        if (tr[k].left == tr[k].right) {
            tr[k].sum = value[left] % P;
            return void();
        }

        const int mid = (tr[k].left + tr[k].right) >> 1;
        const int lc = k << 1, rc = k << 1 | 1;

        build_tree(lc, left, mid);
        build_tree(rc, mid + 1, right);

        push_up(k);
    }

    void modify(const int k, const int left, const int right, const long long value) {
        if (tr[k].left >= left && tr[k].right <= right) {
            make_lazy(k, value);
            return void();
        }

        push_down(k);

        const int mid = (tr[k].left + tr[k].right) >> 1;
        const int lc = k << 1, rc = k << 1 | 1;

        if (right <= mid) {
            modify(lc, left, right, value);
        } else if (left > mid) {
            modify(rc, left, right, value);
        } else {
            modify(lc, left, mid, value);
            modify(rc, mid + 1, right, value);
        }

        push_up(k);
    }

    long long query(const int k, const int left, const int right) {
        if (tr[k].left >= left && tr[k].right <= right) {
            return tr[k].sum % P;
        }

        push_down(k);

        const int mid = (tr[k].left + tr[k].right) >> 1;
        const int lc = k << 1, rc = k << 1 | 1;

        if (right <= mid) {
            return query(lc, left, right) % P;
        }
        if (left > mid) {
            return query(rc, left, right) % P;
        }
        return (query(lc, left, mid) + query(rc, mid + 1, right)) % P;
    }
} seg;

void dfs(int u, int father) {
    fa[u] = father;
    size[u] = 1;
    dep[u] = dep[father] + 1;

    for (int i = head[u]; ~i; i = e[i].next) {
        int v = e[i].to;
        if (v == father) {
            continue;
        }

        dfs(v, u);
        size[u] += size[v];
        if (son[u] == 0 || size[v] > size[son[u]]) {
            son[u] = v;
        }
    }
}

void dfs2(int u, int link_top) {
    top[u] = link_top;
    id[u] = ++num;
    value[num] = initial_value[u];

    if (son[u] == 0) return;
    
    dfs2(son[u], link_top);

    for (int i = head[u]; ~i; i = e[i].next) {
        int v = e[i].to;
        if (v == fa[u] || v == son[u]) {
            continue;
        }

        dfs2(v, v);
    }
}

using std::swap;

void update_range(int u, int v, const long long value) {
    while (top[u] != top[v]) {
        #ifndef OnlineJudge
        cout << u << ' ' << top[u] << " " << v << " " << top[v] << '\n';
        #endif
        
        if (dep[top[u]] < dep[top[v]]) {
            swap(u, v);
        }

        seg.modify(1, id[top[u]], id[u], value);
        u = fa[top[u]];
    }

    if (dep[u] > dep[v]) {
        swap(u, v);
    }
    seg.modify(1, id[u], id[v], value);
}

long long query_range(int u, int v) {
    long long retval = 0;

    while (top[u] != top[v]) {
        if (dep[top[u]] < dep[top[v]]) {
            swap(u, v);
        }

        (retval += seg.query(1, id[top[u]], id[u])) %= P;
        u = fa[top[u]];
    }

    if (dep[u] > dep[v]) {
        swap(u, v);
    }
    (retval += seg.query(1, id[u], id[v])) %= P;

    return retval;
}

void update_subtree(const int u, const long long value) {
    seg.modify(1, id[u], id[u] + size[u] - 1, value);
}

long long query_subtree(const int u) {
    return seg.query(1, id[u], id[u] + size[u] - 1);
}

using std::cin;
using std::cout;

int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);

    std::memset(head, -1, sizeof(head));
    
    cin >> n >> m >> root >> P;

    for (int i = 1; i <= n; i++) {
        cin >> initial_value[i];
    }

    for (int i = 1; i < n; i++) {
        cin >> u >> v;

        add_edge(u, v);
        add_edge(v, u);
    }

    dfs(root, 0);
    dfs2(root, root);

    seg.build_tree(1, 1, n);

    for (int i = 1; i <= m; i++) {
        cin >> opt;

        switch (opt) {
            int x, y;
            long long z;

            case 1:
                cin >> x >> y >> z;
                update_range(x, y, z);
                break;
            case 2:
                cin >> x >> y;
                cout << query_range(x, y) << '\n';
                break;
            case 3:
                cin >> x >> z;
                update_subtree(x, z);
                break;
            case 4:
                cin >> x;
                cout << query_subtree(x) << '\n';
                break;
            default:
                break;
        }
    }

    return 0;
}
```

## 左偏树

左偏树是可并堆的一种。

### 定义

我们定义 **外节点** 表示子节点数小于两个的节点。定义一个节点 $u$ 的 $dist_u$ 表示到最近的外节点的距离。
左偏树满足如下性质：

+ 对于任意一个节点 $u$，均满足 $dist_{\operatorname{lson}(u)} > dist_{\operatorname{rson}(u)}$，即 ***左偏***。
+ 由上一条性质我们可以推导出：$dist_u = dist_{\operatorname{rson}(u)} + 1$。

### 操作

#### 合并

左偏树的合并操作有以下几个步骤：

+ 定义 $\operatorname{merge}(x, y)$ 表示将以 $x$ 和 $y$ 为根节点的左偏树合并。
+ 我们钦定 $val_x < val_y$，将 $x$ 作为合并后的新的根节点 即维护一个小根堆。
+ 接下来进行 $\operatorname{merge}(\operatorname{lson}(x),y)$。
+ 然后维护左偏树的性质：
    + 如果 $dist_{\operatorname{lson}(x)} < dist_{\operatorname{rson}(x)}$，则 $\operatorname{swap}(\operatorname{lson}(x), \operatorname{rson}(x))$。
    + 维护 $dist_x = dist_{\operatorname{rson}(x)} + 1$。

#### 删除

删除还是基于左偏树的 $\operatorname{merge}(x,y)$ 操作，如果删除的是 $x$，则 $\operatorname{merge}(\operatorname{lson}(x), \operatorname{rson}(x))$，然后维护左偏树的性质即可

### 例题

[洛谷 P3337](www.luogu.com.cn/problem/P3337) 左偏树/可并堆

模板题，根据题意维护即可。

**Code**

```c
#include <bits/stdc++.h>

using std::cin;
using std::cout;

constexpr int N = 1e5 + 5;
int n, m, erase[N], op, x, y;

class LeftistTree {
public:
    struct Node {
        int value, index;

        Node(const int v = 0, const int i = 0) :
            value(v), index(i) {}

        bool operator<(const Node& compare_node) const {
            if (value != compare_node.value) {
                return value < compare_node.value;
            }
            return index < compare_node.index;
        }

        bool operator>(const Node& compare_node) const {
            if (value != compare_node.value) {
                return value > compare_node.value;
            }
            return index > compare_node.index;
        }
    } val[N];
    int father[N] = {};
    int dist[N] = {}, lc[N] = {}, rc[N] = {};

    int find(const int x) {
        if (father[x] != x) father[x] = find(father[x]);
        return father[x];
    }

    int merge(int x, int y) {
        using std::swap;

        if (x == 0 || y == 0) return x + y;
        if (val[x] > val[y]) swap(x, y);

        rc[x] = merge(rc[x], y);

        if (dist[lc[x]] < dist[rc[x]]) swap(lc[x], rc[x]);
        dist[x] = dist[rc[x]] + 1;

        return x;
    }
} tr;

int main() {
    using std::memset;

    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);

    memset(tr.rc, 0, sizeof(tr.rc));
    memset(tr.lc, 0, sizeof(tr.lc));
    memset(tr.dist, 0, sizeof(tr.dist));
    tr.dist[0] = -1;


    cin >> n >> m;

    for (int i = 1; i <= n; i++) {
        cin >> tr.val[i].value;
        tr.val[i].index = i;
        tr.father[i] = i;
    }

    for (int i = 1; i <= m; i++) {
        cin >> op;

        if (op == 1) {
            cin >> x >> y;

            if (erase[x] || erase[y]) {
                continue;
            }

            x = tr.find(x), y = tr.find(y);
            if (x == y) {
                continue;
            }

            tr.father[x] = tr.father[y] = tr.merge(x, y);
        } else if (op == 2) {
            cin >> x;

            if (erase[x] == true) {
                cout << "-1\n";
            } else {
                x = tr.find(x);
                cout << tr.val[x].value << '\n';

                erase[x] = true;
                const int current_root = tr.merge(tr.lc[x], tr.rc[x]);
                tr.father[tr.lc[x]] = current_root;
                tr.father[tr.rc[x]] = current_root;
                tr.father[x] = current_root;
                tr.lc[x] = 0;
                tr.rc[x] = 0;
                tr.dist[x] = 0;
            }
        }
    }

    return 0;
}
```

## 点分治

[OI Wiki](https://oi-wiki.org/graph/tree-divide/#%E7%82%B9%E5%88%86%E6%B2%BB)

### 思想

我们对每个节点进行分治，将树上的路径分为 *经过点 $u$ 的* 和 *不经过点 $u$ 的*，而点 $u$ 通常取树的重心。

典型问题：$K$ 长路径计数。