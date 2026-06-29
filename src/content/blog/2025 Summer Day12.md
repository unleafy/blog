---
title: '2025 Summer Day12'
description: '暑期集训 Day12'
date: '2025-07-26T03:22:39.534Z'
draft: false
tags:
  - 比赛
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day12


**Content**：模拟赛

**Date**：2025.7.28



# Problem-A [排序](https://zhengruioi.com/problem/3259?cid=1944)

## 题目大意

优化程序：

```c
#include <algorithm>
#include <cmath>
#include <iostream>

const int N = 3e7 + 5;
int n, seed;
double a[N];

void gen_input(const int n, const int seed) {
  double x = (double)(seed);
  unsigned int iseed = seed;
  for (int i = 0; i < n; ++i) {
    iseed = iseed * 1664525 + 1013904223;
    x = (std::sin(iseed % 4096) + 1) / 2;
    a[i] = std::fabs(x * (iseed >> (iseed % 30))) + 1e-9;
  }
}

int main() {
  std::cin >> n >> seed;
  gen_input(n, seed);

  // 你需要优化这行代码
  std::sort(a, a + n);

  double ans = 0;
  for (int i = 0; i < n; i++) {
    ans += a[i] * (i % 9);
  }
  std::cout.precision(10);
  std::cout << std::fixed << ans << std::endl;
  return 0;
}
```

## 思路

赛场上看到这道题目~~一脸懵~~，到底是哪个出题人出的题啊。

赛后发现这道题其实考察的是浮点数。浮点数由于其特殊的存储特性，比较的常数是特别大的，但是将 *double* 类型转换为 *long long* 类型是不影响比较的，所以只需要添加一行代码：

```c
long long *p = (long long *) (void *) a;
sort(p, p + n);
```

这里就是直接把 $a$ 数组转换为 *long long* 类型进行比较。

# Problem-B [北京折叠](https://zhengruioi.com/problem/3260?cid=1944)

赛时过了，就是一个模拟。

# Problem-C [Git](https://zhengruioi.com/problem/3262?cid=1944)

## 思路

赛场上的贪心策略有问题，当时想的是将子树大小大的接到小的上面，这样做的操作次数会比把小的接到大的上面的操作次数少 (就是重链剖分的思路)。

但是这道题要求是变成一条链，所以其答案一定和深度有关，不难发现将这棵树变为一条链的最小操作次数为 $n - \max_{u} \operatorname{dep}(u)$。所以不应该以重链剖分的思路解这道题，而是长链剖分。其他的思路就和上面一样了。就是不断的把 $u$ 的长链剖分意义下的重儿子接到任意一个轻儿子上 (选择的轻儿子变为下一次的重儿子)，不断重复这个操作，直到 $u$ 不存在轻儿子。

# Problem-D [哈利波特魔法觉醒](https://zhengruioi.com/problem/3261?cid=1944)

## 思路

赛时考虑将每个区间的信息拆开来，即对于每个区间的每个端点维护信息，然后将其按位置排序，然后 $O(n)$ 计算每个时间点的 $k$ 连胜的概率。但是显然不是很好维护。赛时也想到了要 *DP*，但是状态定义为：$dp_i$ 表示到第 $i$ 个时间的 $k$ 连胜的概率，转移不好考虑。

其实对于区间的信息可以 ==**差分**== 维护，再在 *DP* 的时候进行累加还原，即可获得获胜的概率 $p$。

所以我们逆向思维，定义 $dp_i$ 表示以 $i$ 结尾没有 $k$ 连胜的概率。而这个转移比较好考虑。

$$
dp_i =
\begin{cases}
dp_{i-1} - dp_{i-k-1} \times (1-p_{i-k}) \times \prod_{j=0}^{k-1} p_{i-j} & i \ge k \\
1 & otherwise \\
\end{cases} 
$$

即以时间 $i$ 结尾的没有 $k$ 连胜的概率为前 $i-k-1$ 个时刻里没有 $k$ 连胜的概率乘上 $1$ 减去区间 $[i-k+1,i]$ 内恰好 $k$ 连胜的概率。