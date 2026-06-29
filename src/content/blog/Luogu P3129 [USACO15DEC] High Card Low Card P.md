---
title: "Luogu P3129 [USACO15DEC] High Card Low Card P"
description: "Luogu P3129 [USACO15DEC] High Card Low Card P"
date: 2025-6-29T03:22:39.534Z
draft: false
tags:
  - 贪心
series:
  - '题解'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# Luogu P3129 [USACO15DEC] High Card Low Card P


# [P3129](https://www.luogu.com.cn/problem/P3129) [USACO15DEC] High Card Low Card P



## 算法
+ 贪心



## 思路
由于 Bessie 预先知道了 Elsie 的出牌策略, 所以可以贪心, 考虑每次都出比 Elsie 大一点点的牌, 如果没有就改变规则, 这样可以保证得分最大化.
记 $f_i$ 表示每次都出大一点点的牌最多可以赢几次, $g_i$ 相反.
则最后的答案为 $max_{i}^{n} \{f_i + g_{i+1}\}$
关于重复的证明见 [Link](https://www.luogu.com.cn/article/yjmjccb8)



## 代码



```c
#include <bits/stdc++.h>
  
using namespace std;

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  
  int n;
  cin >> n;
  
  vector<int> Elsie(n);
  set<int, greater<int>> BessieG;
  set<int, less<int>> BessieL;
  vector<bool> used(2 * n + 1, false);
  
  for (int i = 0; i < n; i++) {
    cin >> Elsie[i];
  
    used[Elsie[i]] = true;
  }
  
  for (int i = 1; i <= 2 * n; i++) {
    if (used[i] == false) {
      BessieG.insert(i);
      BessieL.insert(i);
    }
  }
  
  vector<int> f(n + 2), g(n + 2);
  
  for (int i = 1; i <= n; i++) {
    set<int>::iterator found = BessieL.lower_bound(Elsie[i - 1]);
  
    // * 如果 found != BessieG.end(), 则 Bessie 可以在这一轮出比 Elsie 大的
    if (found != BessieL.end()) {
      BessieL.erase(found);
      f[i] = f[i - 1] + 1;
    } else {
      f[i] = f[i - 1];
    }
  }
  
  // ! 注意这里要逆序循环 (40 pts)
  for (int i = n; i >= 1; i--) {
    set<int>::iterator found = BessieG.lower_bound(Elsie[i - 1]);
  
    // * 如果 found != BessieL.end(), 则 Bessie 可以在这一轮出比 Elsie 小的
    if (found != BessieG.end()) {
      BessieG.erase(found);
      g[i] = g[i + 1] + 1;
    } else {
      g[i] = g[i + 1];
    }
  }
  
  int answer = 0;
  
  // ! 这里统计答案的范围为 [0, n] (47 pts)
  for (int i = 0; i <= n; i++) {
    answer = max(answer, f[i] + g[i + 1]);
  }
  
  cout << answer << '\n';
  
  return 0;
}
```