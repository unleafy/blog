---
title: "P1445 [Violet] 樱花"
description: "P1445 [Violet] 樱花"
date: 2025-7-3T03:22:39.534Z
draft: false
tags:
  - 数学
series:
  - '题解'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# P1445 [Violet] 樱花


## 算法
+ 数学（素数筛，因子个数）



## 思路
对于原式做如下推导：

$$
\large
\begin{aligned}
\frac{1}{x} + \frac{1}{y} &= \frac{1}{n!} \\
\frac{x + y}{xy} &= \frac{1}{n!} \\
n! \cdot (x + y) &= xy \\
n! \cdot x + n! \cdot y - xy &= 0 \\
xy - n! \cdot x - n! \cdot y &= 0
\end{aligned}
$$

由上式联想到：

$$
\large
(a - x) (b - x) \to ab - ax - bx - X^2
$$

比较两式，发现缺少 $(n!)^2$ 项。加上 $(n!)^2$，得：

$$
\large
\begin{aligned}
(x - n!)(y - n!) = (n!)^2
\end{aligned}
$$

由于等式左边是乘积的形式，所以 $(x - n!)$ 和 $(y - n!)$ 必定是 $(n!)^2$ 的一对因子。
将 $n$ 质因数分解：

$$
\large
n = \prod_{i = 0}^{k} p_i^{\alpha_{i}}
$$

对于 $n!$ 的因子个数 $sum_{1}$ 有：

$$
sum_{1} = \prod_{i=0}^{k} (\alpha_{i}+ 1)
$$

而 $(n!)^2$ 的因子个数 $sum_{2}$ 为：

$$
sum_{2} = \prod_{i = 0}^{k} (\alpha_{1} \cdot 2 + 1)
$$

根据乘法原理，最后的答案为所有 $sum_{2}$ 的乘积。



## 代码
```c
#include <bits/stdc++.h>

using namespace std;

constexpr int MOD = 1e9 + 7;

inline vector<int> GetPrimes(int Limit) {
  vector<int> primes;
  vector<bool> mark(Limit + 1);
  
  for (int i = 2; i <= Limit; i ++) {
    if (mark[i] == false) {
      primes.emplace_back(i);
    }
    
    for (int j = 0; i * primes[j] <= Limit; j ++) {
      mark[i * primes[j]] = true;
      if (i % primes[j] == 0) {
        break;
      }
    }
  }
  
  return primes;
}

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  
  int n;
  cin >> n;
  
  vector<int> primes = GetPrimes(n);
  // * 注意由于是连乘，所以 answer 的初始值为 1.
  long long answer = 1;
  
  for (auto&& p : primes) {
    int alpha = 0, number = n;
    
    while (number) {
      alpha += number / p;
      number /= p;
    }
    
    answer = (answer * (alpha + alpha + 1) % MOD) % MOD;
  }
  
  cout << answer % MOD << '\n';
  
  return 0;
}
```