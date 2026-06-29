---
title: '2025 Summer Day11'
description: '暑期集训 Day11'
date: '2025-07-25T03:22:39.534Z'
draft: false
tags:
  - DP
  - 矩阵
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day11


**Content**：矩阵 DP

**Date**：2025.7.27




# Review

矩阵基本操作：[link](https://fallenleaf0327.github.io/2025/07/20/OI%E9%9B%86%E8%AE%AD-Day4/)

# Example1 - [洛谷-P1962](https://www.luoogu.com.cn/problem/P1962) 斐波那契数列


## 题目描述

给定 $n$，求斐波那契数列的第 $n$ 项 $f_n$。

$$
f_n = 
 \begin{cases}
 1 & n = 0,1 \\
 f_{n-1} + f_{n-2} & oterwise
 \end{cases}
$$

数据范围：$n < 2^{63}$。


## 思路

首先显然 $O(n)$ 的递推是不行的了，我们考虑将递推式转化为矩阵乘法的形式：

$$
\begin{aligned}
\begin{bmatrix}
f_{n - 1} \\
f_{n - 2}
\end{bmatrix}
\times
\begin{bmatrix}
1 & 1 \\
0 & 1 \\
\end{bmatrix}
&=
\begin{bmatrix}
f_{n} \\
f_{n - 1}
\end{bmatrix} \\
\\
\begin{bmatrix}
f_{n - 2} \\
f_{n - 3}
\end{bmatrix}
\times
\begin{bmatrix}
1 & 1 \\
0 & 1 \\
\end{bmatrix}^2
&=
\begin{bmatrix}
f_{n} \\
f_{n - 1}
\end{bmatrix} \\

&\dots \\

\begin{bmatrix}
f_{0} \\
f_{1} \\
\end{bmatrix}

\times 

\begin{bmatrix}
1 & 1 \\
0 & 1
\end{bmatrix}^{n-2}

&=

\begin{bmatrix}
f_{n}
f_{n - 1}
\end{bmatrix}

\end{aligned}
$$

这里只需要矩阵快速幂即可。复杂度 $O(log n)$。

提交记录：[link](https://www.luogu.com.cn/record/227152084)

# Example2 - [UVA11270](https://vjudge.net/problem/UVA-11270) Tiling Dominoes

> **题目描述**
> 给定一个 $n \times m$ 的网格，求用 $1 \times 2$ 的方块覆盖网格的方案数。
> 数据范围：$n \times m \le 100$。

## 思路

我们考虑轮廓线 DP，定义 $dp_{i,j,s}$ 表示当前修改的点是 $(i,j)$，其状态为 $s$。这里的 $s$ 和之前题目里的不同，它表示的是第 $i$ 行的前 $j-1$ 列的覆盖情况和第 $i - 1$ 行的后 $m - j + 1$ 列的覆盖情况，其中 $0$ 表示还未被覆盖，$1$ 表示已经被覆盖。

$dp$ 的第一维可以省略，所以转化为 $dp_{i,s}$ 表示考虑到第 $j$ 列，其状态为 $s$ 的方案数。

接下来考虑转移，设上一行的状态为 $s$，转移有三种。

1. 当前位置不放，留空 (前提条件：$s >> j \ \& \ 1$)：$dp_{i-1,s} \to dp_{i,s \oplus (1 << j)}$
2. 当前位置横着放 (前提条件：$j > 0$ 且 $s >> j \ \& \ 1$)：$dp_{i-1,s} \to dp_{i,s | (1 << (j - 1))}$
3. 当前位置竖着放 (前提条件：$i > 0$ 且 $!(s >> j \& 1)$)：$dp_{i-1,s \oplus (1 << j)} \to dp_{i, s}$。

这样就写完了。

## Code

```c
#include <bits/stdc++.h>

using std::cin;
using std::cout;

constexpr int N = 105;
int n, m;
long long dp[2][1 << 11];

int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    
    while (cin >> n >> m) {
        if (n < m) std::swap(n, m);
        
        int line = 0, max_status = 1 << m;
        std::memset(dp, 0, sizeof(dp));
        dp[line][max_status - 1] = 1;
        
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < m; j++) {
                line = !line;
                std::memset(dp[line], 0, sizeof(dp[line]));
                
                for (int status = 0; status < max_status; status++) {
                    if (status >> j & 1) {
                        dp[line][status ^ (1 << j)] += dp[!line][status];
                    }
                    if (j > 0 && (status >> (j - 1) & 1) == 0 && (status >> j & 1)) {
                        dp[line][status | (1 << (j - 1))] += dp[!line][status];
                    }
                    if (i > 0 && (status >> j & 1) == 0) {
                        dp[line][status | (1 << j)] += dp[!line][status];
                    }
                }
            }
        }
        
        cout << dp[line][max_status - 1] << '\n';
    }
    
    return 0;
}
```

# Example3 - [洛谷-P5678](https://www.luogu.com.cn/problem/P5678) 河神

## 思路

我们一样考虑将递推式转化成矩阵乘法的形式。

但是这里我们注意到地推中的操作是 $|$ 和 $\&$，所以哦我们要做 $(|,\&)$ 矩阵乘法 (这个需要证明 $|$ 操作对 $\&$ 操作具有分配律，这里就不证明了)。

$$
\begin{bmatrix}
a_{n - 1} & a_{n-2} & a_{n-3} & \dots & a_{n-k}
\end{bmatrix}
\times
\begin{bmatrix}
b_{n-1} & b_{n - 2} & b_{n - 3} & \dots & b_1 & b_0 \\
-1 & 0 & 0 & \dots & 0 & 0 \\
0 & -1 & 0 & \dots & 0 & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots & \vdots \\
0 & 0 & 0 & \dots & -1 & 0
\end{bmatrix}
=
\begin{bmatrix}
a_n & a_{n - 1} & a_{n - 2} & \dots & a_{n - k + 1}
\end{bmatrix}
$$

所以就可以使用矩阵快速幂了。

提交记录：[link](https://www.luogu.com.cn/record/227249743)。