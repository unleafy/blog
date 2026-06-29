---
title: '2025 Summer Day21'
description: '暑期集训 Day21'
date: '2025-08-02T03:22:39.534Z'
draft: false
tags: []
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---


# 2025 Summer Day21


# [Luogu-P13270](https://www.luogu.com.cn/problem/P13270) 最小表示法

## 题意

给定一个长度为 $n$ 的字符串 $s$，求 $s$ 的最小表示法。

## 思路

首先如果暴力的话，复杂度是 $O(n^2)$ 的，但是我们发现如果我们当前已经发现了一个最小表示法，我们想要找到一个比这个还小的表示法，那么一定存在一个位置，使得这个位置上的字符不相等，根据这个就可以把复杂度优化到 $O(n)$。

提交记录：[Link](https://www.luogu.com.cn/record/229168015)。

# [Luogu-P5357](https://www.luogu.com.cn/problem/P5357) AC自动机

## 题意

给定一个字符串 $s$ 和 $n$ 个模式串 $t_i$，求每个模式串在字符串 $s$ 中的出现次数。

## 思路

首先我们可以对 $s$ 建一个 AC 自动机，然后统计每个模式串在这个 AC 自动机的字典树上经过了那些节点，然后打标记。再建出 Fail 树，然后在 Fail 树上对标记求和。

提交记录：[Link](https://www.luogu.com.cn/record/229191297)。

# 模拟题 - 嗑瓜子

## 题意

有一个人在嗑 $n$ 个瓜子，每次在瓜子中等概率拿出一个，有以下两种情况：
1. 如果拿出的是瓜子，则吃掉，并把壳扔回瓜子中。
2. 如果拿出的是壳，则将其丢弃。
求他吃掉所有瓜子的期望。

## 思路

定义 $dp_{i,j}$ 表示有 $i$ 个瓜子和 $j$ 个壳的期望。根据题意写出转移即可。

***Important***：注意空间，因为 $j_{max}$ 可以达到 $2 \times n$，所以第二维要开到两倍。

## Code

```c
#include <iostream>
#define OnlineJudge

using std::cin;
using std::cout;

constexpr int MOD = 998244353;
constexpr int N = 4005;    // ! 注意两倍空间
long long dp[N][N];

long long QuickPow(long long base, long long pow) {
    long long result = 1;
    while (pow) {
        if (pow & 1) result = result * base % MOD;
        base = base * base % MOD;
        pow >>= 1;
    }
    return result;
}

int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    
    int n;
    cin >> n;
    
    dp[n][0] = 1;
    
    for (int i = n - 1; i >= 0; i--) {
        for (int j = (n - i) * 2; j >= (i == 0 ? 2 : 0); j--) {
            if (i == 0) {
                dp[i][j] = QuickPow(j - 1, MOD - 2) * dp[i + 1][j - 2] % MOD;
            } else {
                if (j < 2) {
                    dp[i][j] = (j + 1) * QuickPow(i + j + 1, MOD - 2) % MOD * dp[i][j + 1] % MOD;
                } else if (j == (n - i) * 2) {
                    dp[i][j] = (i + 1) * QuickPow(i + j - 1, MOD - 2) % MOD * dp[i + 1][j - 2] % MOD;
                } else {
                    dp[i][j] = ((i + 1) * QuickPow(i + j - 1, MOD - 2) % MOD * dp[i + 1][j - 2] % MOD + (j + 1) * QuickPow(i + j + 1, MOD - 2) % MOD * dp[i][j + 1] % MOD) % MOD;
                }
            }
        }
    }
    
    #ifndef OnlineJudge
    for (int i = 0; i <= n; i++) {
        for (int j = 0; j <= 2 * (n - i); j++) {
            cout << dp[i][j] << ' ';
        }
        cout << '\n';
    }
    #endif
    
    long long answer = 0;
    for (int i = 1; i <= n * 2; i++) {
        answer = (answer + (3 * n - i) * dp[0][i] % MOD) % MOD;
    }
    
    cout << answer << '\n';
    
    return 0;
}
```