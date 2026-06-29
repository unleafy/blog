---
title: "CF727F Polycarp's problems"
description: "CF727F Polycarp's problems"
date: 2025-6-25T03:22:39.534Z
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

# CF727F Polycarp's problems


## 算法
+ 贪心



## 思路
考虑特殊情况。当 $m=1$ 时，问题转化为：给定 $a_0$, 求删除最少元素使得对于任意的 $i$，满足 $\Sigma_{j=0}^{i} a_{j} \ge 0$。
将特殊情况扩展到 $m \le 10^6$，即对于每一个给定的 $a_0$，求解上述问题。
考虑贪心，即对于一个 $i$，满足 $\Sigma_{j=0}^{i} a_{j} < 0$， 则必定删除前面最大的负数，才会使最后的结果最优.于是可以逆向思维，倒序遍历数组 $a$，维护一个大根堆(即维护绝对值最小的负数，对于每一个 $a_{i} < 0$, 将其入队；对于每一个 $a_{i} \ge 0$，用 $a_{i}$ 抵消堆顶元素，直到堆为空或 $a_{i} < 0$。
最后将堆里的元素全部放入一个新数组，即这些负数在 $a_{1} \to a_{n}$ 里都不能被抵消，只能由 $a_{0}$ 抵消，被删除的个数即为剩余负数的个数减去 $a_{0}$ 最多可以抵消的负数，可以二分解决。



## 代码


```c
#include <bits/stdc++.h>
#define OnlineJudge
  
using namespace std;
int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  
  int n, m;
  cin >> n >> m;
  
  vector<long long> a(n), b(m);

  for (int i = 0; i < n; i++) {
    cin >> a[i];
  }

  priority_queue<long long, vector<long long>, less<long long>> q;

  for (int i = n - 1; i >= 0; i--) {
    if (a[i] < 0) {
      q.emplace(a[i]);
    } else {
      while (!q.empty() && a[i] >= 0) {
        a[i] += q.top();
        q.pop();
      }
      
      if (a[i] < 0) {
        q.emplace(a[i]);
      }
    }
    continue;
  }
  
  vector<long long> modify;
  
  while (!q.empty()) {
    modify.emplace_back(-q.top());
    q.pop();
  }

  // reverse(modify.begin(), modify.end());

  for (int i = 1; i < static_cast<int>(modify.size()); i++) {
    modify[i] = modify[i] + modify[i - 1];
  }

  #ifndef OnlineJudge
  for (auto&& var : modify) {
    cout << var << " ";
  }
  cout << "\n";
  #endif

  for (int i = 0; i < m; i++) {
    cin >> b[i];

    int answer;
    if (modify.empty() || modify.back() <= b[i]) {
      answer = 0;
    } else {
      int pos = upper_bound(modify.begin(), modify.end(), b[i]) - modify.begin();
      answer = static_cast<int>(modify.size()) - pos;
    }

    cout << answer << '\n';
  }

  return 0;
}
```