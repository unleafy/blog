---
title: '2025 Summer Day8'
description: '暑期集训 Day8'
date: '2025-07-22T03:22:39.534Z'
draft: false
tags:
  - DP
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day8


**Content**：DP (Interval, Tree)

**Date**：2025.7.24



# 概览

+ 区间 DP
+ 树形 DP

# 例题

## [洛谷-P4516](https://www.luogu.com.cn/problem/P4516) 潜入行动

### 题目大意


**题目大意**
> 给定一颗树，要求在树上选取恰好 $\large k$ 个节点 (不得重复)，每个选取的节点可以覆盖它的邻居，**但是不能覆盖自己本身**。要求选取的这 $\large k$ 个节点覆盖所有的 $\large n$ 个节点。求方案数。
数据范围 $\large n \le 10^5, k \le 100$。


### 思路

我们考虑树形动态规划。定义状态 $\large f_{u,i,0/1,0/1}$ 表示以 $\large u$ 为根的子树内，选取了 $\large i$ 个节点，其中点 $\large u$ 放了/不放，被/不被覆盖的方案数。初始状态为：

$$
\large 
\begin{aligned}
f_{u,0,0,0} = 1 \\
f_{u,1,1,0} = 1 \\
\end{aligned}
$$

可以发现如果只看前两维的话，是一个典型的树形背包问题。接下来我们考虑加上后两维后如何转移 (本质还是树形背包)。

对于 $\large f_{u,i,0,0}$，即点 $\large u$ 既不选择，也 不覆盖，所以 $u$ 的子节点 $v$ 一定不能选择，而 $v$ 必须     覆盖，所以其转移如下：
    
$$
    \large
    f_{u,i,0,0} = \sum_{v \in \operatorname{son}(u)} f_{u,i - j,0,0} \times f_{v,j,0,1}
$$
    
对于 $\large f_{u,i,0,1}$，即点 $\large u$ 不选择，但是被覆盖。因为状态为点 $u$ 被覆盖，所以既可以是 $v$ 覆盖的，也可能是 $u$ 的其他子节点覆盖的，而因为 $u$ 不被选择，所以 $v$ 一定是已经被覆盖了的，所以其转移如下：
    
$$
    \large 
    f_{u,i,0,1} = \sum_{v \in \operatorname{son}(u)} f_{u,i-j,0,1} \times (f_{v,j,0,1} + f_{v,j,1,1}) + \sum_{v \in \operatorname{son}(u)} f_{u,i-j,0,0} \times f_{v,j,1,1}
$$
    
对于 $\large f_{u,i,1,0}$，即点 $\large u$ 选择了，但还没有被覆盖。因为不被覆盖，所以点 $\large v$ 一定不能被选择，而点 $\large u$ 已经被选择了，所以点 $\large v$ 覆不覆盖无所谓。转移如下：
    
$$
    \large 
    f_{u,i,1,0} = \sum_{v \in \operatorname{son}(u)} f_{u,i-j,1,0} \times (f_{v,j,0,1} + f_{v,j,0,0})
$$
    
对于 $\large f_{u,i,1,1}$，即点 $\large u$ 选择了，也被覆盖了的情况。由 $1,3$ 两种情况类似的推导就可以得到。转移为：
    
$$
    \large 
    f_{u,i,1,1} = \sum_{v \in \operatorname{son}(u)} f_{u,i-j,1,1} \times (f_{v,j,0,0} + f_{v,j,0,1} + f_{v,j,1,0} + f_{v,j,1,1}) + \sum_{v \in \operatorname{son}(u)} f_{u,i-j,1,0} \times (f_{v,j,1,0} + f_{v,j,1,1})
$$

最后，注意 **取模** 和 **long long**!!!

### Code

```c
#include <bits/stdc++.h>

namespace IO {
    template<typename name> name read() {
        name x = 0, f = 1; char ch = getchar();
        while (!isdigit(ch)) { if (ch == '-') f = -1; ch = getchar(); }
        while (isdigit(ch)) { x = (x << 1) + (x << 3) + (ch ^ 48); ch = getchar(); }
        return x * f;
    }
    template<typename name> void _write(name x) {
        if (x > 9) _write(x / 10);
        putchar(x % 10 + '0');
    }
    template<typename name> void write(name x) {
        if (x < 0) putchar('-'), x = -x;
        _write(x);
    }
}

constexpr int N = 1e5 + 5, K = 105, MOD = 1e9 + 7;
int head[N], next[N << 1], to[N << 1], cnt = 0;
int dp[N][K][2][2], size[N], tmp[K][2][2];
int n, k, u, v;

inline void AddEdge(const int u, const int v) {
    to[cnt] = v;
    next[cnt] = head[u];
    head[u] = cnt++;
}

inline void Dfs(int u, int father) {
    dp[u][0][0][0] = dp[u][1][1][0] = 1;
    size[u] = 1;
    
    for (int i = head[u]; ~i; i = next[i]) {
        const int v = to[i];
        if (v == father) continue;
        
        Dfs(v, u);
        size[u] += size[v];
        
        // ! 这里要进行备份，不然转移的时候会出现问题（用已修改的状态修改未修改的状态）
        for (int t = 0; t <= std::min(k, size[u]); t++) {
            tmp[t][0][0] = dp[u][t][0][0];
            tmp[t][1][0] = dp[u][t][1][0];
            tmp[t][0][1] = dp[u][t][0][1];
            tmp[t][1][1] = dp[u][t][1][1];
            dp[u][t][0][0] = dp[u][t][1][0] = dp[u][t][0][1] = dp[u][t][1][1] = 0;
        }
        
        // ! 注意枚举范围，不能直接枚举区间 [0,k]，不然会 TLE
        for (int t = 0; t <= std::min(size[u], k); t++) {
            // ! 这里同理
            for (int j = 0; j <= std::min(size[v], t); j++) {
                if (t - j > size[u] - size[v]) continue;
                
                // // 可恶的转移
                (dp[u][t][0][0] += (long long) tmp[t - j][0][0] * dp[v][j][0][1] % MOD) %= MOD;
                (dp[u][t][0][1] += ((long long) tmp[t - j][0][1] * ((dp[v][j][0][1] + dp[v][j][1][1]) % MOD) % MOD + (long long) tmp[t - j][0][0] * dp[v][j][1][1] % MOD) % MOD) %= MOD;
                (dp[u][t][1][0] += (long long) tmp[t - j][1][0] * ((dp[v][j][0][1] + dp[v][j][0][0]) % MOD) % MOD) %= MOD;
                (dp[u][t][1][1] += ((long long) (tmp[t - j][1][1] * (((long long) dp[v][j][0][1] + dp[v][j][0][0] + dp[v][j][1][0] + dp[v][j][1][1]) % MOD) % MOD) + (long long) tmp[t - j][1][0] * ((dp[v][j][1][1] + dp[v][j][1][0]) % MOD) % MOD) % MOD) %= MOD;
            }
        }
    }
}

int main() {
    using namespace IO;
    
    n = read<int>();
    k = read<int>();
    
    for (int i = 1; i <= n; i++) head[i] = -1;
    
    for (int i = 1; i < n; i++) {
        u = read<int>(), v = read<int>();
        AddEdge(u, v);
        AddEdge(v, u);
    }
    
    Dfs(1, 1);
    
    write((1ll * dp[1][k][0][1] + dp[1][k][1][1]) % MOD);
    
    return 0;
}
```

## [洛谷-P1352](https://www.luogu.com.cn/problem/P1342) 没有上司的舞会 (经典题)

### 题目大意

**题目大意**
> 给定一颗有 $n$ 个节点的树，表示员工的架构，现在有一个舞会需要举办，你要选取一些人参加，第 $\large i$ 个人参加可以为舞会带来 $\large r_i$ 的快乐值，但是如果第 $\large i$ 个人的直接上司 ($\large i$ 的父亲节点) 参加了，那么 $\large i$ 就不会参加，你要最大化选择的人的快乐值之和。
数据范围：$\large n \le 6 \times 10^3$。

### 思路

定义状态 $\large f_{u,0/1}$ 表示节点 $\large u$ 参加/不参加能为舞会带来的最大价值。

转移如下：

$$
\large 
\begin{aligned}
f_{u,0} &= \sum_{v \in \operatorname{son}(u)} \max(f_{v,0}, f_{v,1}) \\
f_{u,1} &= \sum_{v \in \operatorname{son}(u)} f_{v,0} + r_i
\end{aligned}
$$

注意我的树存的是双向边，和边有关的数组要开 **$\large 2$ 倍**。

### Code

**Code**
```c
#include <bits/stdc++.h>

using std::cin;
using std::cout;

constexpr int MAXN = 6e4 + 5;
int head[MAXN], to[MAXN], next[MAXN], cnt = 0;
int n, r[MAXN], u, v;
int dp[MAXN][2];

void add_edge(const int u, const int v) {
    to[cnt] = v;
    next[cnt] = head[u];
    head[u] = cnt++;
}

void dfs(const int u, const int father) {
    dp[u][1] = r[u];
    
    for (int i = head[u]; ~i; i = next[i]) {
        const int v = to[i];
        if (v == father) continue;
        
        dfs(v, u);
        dp[u][0] += std::max(dp[v][0], dp[v][1]);
        dp[u][1] += dp[v][0];
    }
}

int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);

    std::memset(head, -1, sizeof(head));
    
    cin >> n;
    
    for (int i = 1; i <= n; i++) {
        cin >> r[i];
    }
    
    for (int i = 1; i < n; i++) {
        cin >> u >> v;
        add_edge(u, v);
        add_edge(v, u);
    }
    
    dfs(1, 1);
    
    cout << std::max(dp[1][0], dp[1][1]) << '\n';
    
    return 0;
}
```

## [洛谷-P3478](https://www.luogu.com.cn/problem/P3478) 消防局的 STA-Station

### 题目大意

**P3478 [POI 2008] STA-Station 题目大意**
> 给定一个 $\large n$ 个点的树，请求出一个结点，使得以这个结点为根时，所有结点的深度之和最大。
> 一个结点的深度之定义为该节点到根的简单路径上边的数量。
> 数据范围：$\large n \le 10^6$.

### 思路

换根 DP。设 $\large f_u$ 表示以 $\large u$ 为根节点的时候所有节点的深度之和是多少。换根一般都有两个 $\large dfs$，我们在第一遍 $\large dfs$ 的时候记录节点的深度 $\large dep$，子树大小 $\large size$，然后 $\large f_u$ 的状态为：$\large f_u = dep_u$。

在第二遍 $dfs$ 时，我们自上而下更新 $f_v$ 的值，转移为：

$$
\large 
f_v = f_u - size_v + n - size_v
$$

即子树 $v$ 内所有点的深度减一，子树外的所有深度加一。

注意 *long long*!!!

### Code

**Code**

```c
#include <bits/stdc++.h>

using std::cin;
using std::cout;

constexpr int N = 1e6 + 5;
int head[N], next[N << 1], to[N << 1], cnt = 0;
int n, u, v, dp[N], dep[N], size[N];

void AddEdge(const int u, const int v) {
    to[cnt] = v;
    next[cnt] = head[u];
    head[u] = cnt++;
}

void Dfs1(const int u, const int father) {
    dp[u] += dep[u];
    dep[u] = dep[father] + 1;
    size[N] = 1;
    
    for (register int i = head[u]; ~i; i = next[i]) {
        register const int v = to[i];
        if (v == father) continue;
        
        Dfs1(v, u);
        size[u] += size[v];
    }
}

void Dfs2(const int u, const int father) {
    for (register int i = head[u]; ~i; i = next[i]) {
        register const int v = to[i];
        if (v == father) continue;
        
        dp[v] = dp[u] - size[v] + n - size[v];
        Dfs2(v, u);
    }
}



int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
 
    std::memset(head, -1, sizeof(head));
    
    cin >> n;
    
    for (int i = 1; i < n; i++) {
        cin >> u >> v;
        
        AddEdge(u, v);
        AddEdge(v, u);
    }
    
    Dfs1(1, 1);
    Dfs2(1, 1);
    
    int ans = 1;
    for (int i = 2; i <= n; i++) {
        if (dp[i] > dp[ans]) ans = i;
    }
    
    cout << ans << '\n';
    
    return 0;
}
```