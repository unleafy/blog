---
title: '2025 Summer Day1'
description: '暑期集训 Day1'
date: '2025-07-15T03:22:39.534Z'
draft: false
tags: 
  - 数据结构
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day1


**Content**：Data Structs

**Date：2025.7.17**



## 内容
+ 并查集
+ ST表
+ 线段树

## 关于树状数组

### 一维树状数组

#### 单点修改，区间查询

对于这一类最普通的树状数组，没有什么好说的，直接维护前缀和即可。

```c
struct BIT {
  long long tr[N];
  
  static int lowbit(int x) {
    return x & (-x);
  }
  
  void add(int pos, long long value) {
    for (int i = pos; i <= n; i += lowbit(i))
      tr[i] += value;
  }
  
  long long query(int pos) {
    long long retval = 0;
    for (int i = pos; i > 0; i -= lowbit(i))
      retval += tr[i];
    return retval;
  }
} tr;
```

#### 区间修改，单点查询

这里就需要使用到差分了。
记 $d[i] = arr[i] - arr[i - 1]$，则我们有：

$$
arr[i] = \sum_{i=0}^{n} d[i]
$$

后面的求和可以用树状数组维护。

```c
struct BIT {
  long long tr[N];
  
  static int lowbit(int x) {
    return x & (-x);
  }
  
  void add(int pos, int value) {
    for (int i = pos; i <= n; i += lowbit(i))
      tr[i] += value;
  }
  
  long long query(int pos) {
    long long retval = 0;
    for (int i = pos; i > 0; i -= lowbit(i))
      retval += tr[i];
    return retval;
  }
} tr;
```

这一部分的代码和上面的没什么区别，区别只有 $main()$ 函数中的输入和修改。
对于输入：

```c
for (int i = 1; i <= n; i++) {
  cin >> a[i];
  // 这里存储的是差分后的结果，而不是 a[i]
  tr.add(i, a[i] - a[i - 1]);
}
```

对于修改：

```c
      int l, r, x;
      cin >> l >> r >> x;
      
      // 差分数组上的区间价相当于 d[l] += v, d[r + 1] -= v;
      tr.add(l, x);
      tr.add(r + 1, -x);
```

#### 区间修改，区间查询
对于区间修改，我们沿用上面的思路，接下来我们看如何区间查询。
差分有如下性质：

$$
arr[i] = \sum_{i = 0}^{n} d[i]
$$

而我们要求的区间和可以转化为：

$$
\begin{aligned}
\sum_{i = l}^{r} arr[i] = \sum_{i = 0}^{r} arr[i] - \sum_{i = 0}^{l - 1} arr[i]
\end{aligned}
$$

所以我们的问题重新转化为了如何求解前缀和。

$$
\begin{aligned}
\sum_{i = 0}^{r} arr[i] &= \sum_{i = 0}^{r} \sum_{j = 0}^{i} d[j] \\
&= \sum_{i = 0}^{r} (r - i + 1) \cdot d[i] \\
&= \sum_{i = 0}^{r} (r + 1) \cdot d[i] - i \cdot d[i] \\
&= (r + 1) \cdot \sum_{i = 0}^{r} d[i] - \sum_{i = 0}^{r} d[i] \cdot i \\
\end{aligned}
$$

所以我们用两个树状数组 $tr1, tr2$ 来分别维护两个和式，就解决了区间求和的问题。

```c
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6 + 5;
int n, q, op, a[N];

int lowbit(int x) { return x & (-x); }

struct BIT {
  long long tr[N];

  void add(int pos, long long value) {
    for (int i = pos; i <= n; i += lowbit(i)) tr[i] += value;
  }

  long long query(int pos) {
    long long retval = 0;
    for (int i = pos; i > 0; i -= lowbit(i)) retval += tr[i];
    return retval;
  }
} tr1, tr2;

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);

  cin >> n >> q;

  for (int i = 1; i <= n; i++) {
    cin >> a[i];

    tr1.add(i, a[i] - a[i - 1]);
    tr2.add(i, 1ll * (a[i] - a[i - 1]) * i);
  }

  for (int i = 1; i <= q; i++) {
    cin >> op;

    if (op == 1) {
      int l, r, value;
      cin >> l >> r >> value;
      tr1.add(l, value);
      tr1.add(r + 1, -value);
      tr2.add(l, 1ll * value * l);
      tr2.add(r + 1, -1ll * value * (r + 1));
    } else if (op == 2) {
      int l, r;
      cin >> l >> r;
      
      long long ans = (tr1.query(r) * (r + 1) - tr2.query(r)) - (tr1.query(l - 1) * l - tr2.query(l - 1));
      
      cout << ans << '\n';
    }
  }

  return 0;
}
```

### 二维树状数组

#### Pre：二维前缀和

对于二维数组的前缀和有如下公式：

$$
sum[i][j] = sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1] + arr[i][j]
$$

对于左上角坐标为 $(a, b)$，右下角坐标为 $(c, d)$ 的子矩阵，其和为：

$$
sum[c][d] - sum[a - 1][d] - sum[c][b - 1] + sum[a - 1][b - 1]
$$

#### 单点修改，区间查询

和一维树状数组类似，直接记 $tr[i][j]$ 为左上角坐标为 $(0, 0)$，右下角坐标为 $(i, j)$ 的矩阵的和。

修改和查询直接套公式。

```c
#include <bits/stdc++.h>

using namespace std;

const int N = 1 << 12 + 5;
int n, m, op;

struct BIT {
  vector<vector<long long>> tr;
  
  BIT() = default;
  
  BIT(int n, int m) {
    tr.assign(n + 1, vector<long long>(m + 1, 0ll));
  }
  
  static int lowbit(int x) {
    return x & (-x);
  }
  
  void add(int x, int y, long long value) {
    for (int i = x; i <= n; i += lowbit(i))
      for (int j = y; j <= m; j += lowbit(j))
        tr[i][j] += value;
  }
  
  long long query(int x, int y) {
    long long retval = 0;
    for (int i = x; i > 0; i -= lowbit(i))
      for (int j = y; j > 0; j -= lowbit(j))
        retval += tr[i][j];
    return retval;
  }
} tr;

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  
  cin >> n >> m;
  
  tr = BIT(n, m);
  
  while (cin >> op) {
    if (op == 1) {
      int x, y, value;
      cin >> x >> y >> value;
      
      tr.add(x, y, value);
    } else if (op == 2) {
      int a, b, c, d;
      
      cin >> a >> b >> c >> d;
      
      long long ans = tr.query(c, d) - tr.query(a - 1, d) - tr.query(c, b - 1) + tr.query(a - 1, b - 1);
      
      cout << ans << '\n';
    }
  }
  
  return 0;
}
```

#### 区间修改，单点查询

我们由一维树状数组的区间修改，单点查询启发，可以考虑定义二维差分。
定义二维差分数组（为避免名字冲突，这里定义为 $c[i][j]$）$c[i][j] = a[i][j] - a[i - 1][j] - a[i][j - 1] - a[i - 1][j - 1]$。


**证明**

观察到：
  
$$
  arr[i][j] = sum[i][j] - sum[i - 1][j] - sum[i][j - 1] + sum[i - 1][j - 1]
$$
  
所以二维差分数组 $d[i][j]$ 满足：
  
$$
  a[i][j] = a[i - 1][j] + a[i][j - 1] - a[i - 1][j - 1] + c[i][j]
$$

即：

$$
  c[i][j] = a[i][j] - a[i - 1][j] - a[i][j - 1] + a[i - 1][j - 1]
$$


所以我们得到了二维差分数组 $c[i][j]$ 的表达式。

跟一维差分类似地，我们可以得到子矩阵 $(a, b)$，$(c, d)$ 加法在差分数组中的等效替代：

$$
\begin{aligned}
sum(a, b, c, d, v) \iff & c[a][b] + v, c[c + 1][b] - v, \\
& c[a][d + 1] - v, c[c + 1][d + 1] + v
\end{aligned}
$$

代码如下：
```cpp
#include <bits/stdc++.h>

using namespace std;

int n, m, op;

struct BIT {
  vector<vector<long long>> tr;
  
  BIT() = default;
  
  BIT(int n, int m) {
    tr.assign(n + 1, vector<long long>(m + 1, 0ll));
  }
  
  static int lowbit(int x) {
    return x & (-x);
  }
  
  void add(int x, int y, int value) {
    for (int i = x; i <= n; i += lowbit(i))
      for (int j = y; j <= m; j += lowbit(j))
        tr[i][j] += value;
  }
  
  long long query(int x, int y) {
    long long retval = 0;
    for (int i = x; i > 0; i -= lowbit(i))
      for (int j = y; j > 0; j -= lowbit(j))
        retval += tr[i][j];
    return retval;
  }
} tr;

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  
  cin >> n >> m;
  
  tr = BIT(n, m);
  
  while (cin >> op) {
    if (op == 1) {
      int a, b, c, d, value;
      cin >> a >> b >> c >> d >> value;
      
      tr.add(a, b, value);
      tr.add(a, d + 1, -value);
      tr.add(c + 1, b, -value);
      tr.add(c + 1, d + 1, value);
    } else if (op == 2) {
      int x, y;
      cin >> x >> y;
      
      cout << tr.query(x, y) << '\n';
    }
  }
  
  return 0;
}
```
#### 区间修改，区间查询

保留上面二维树状数组上区间加的操作，接下来看看如何实现区间查询。
对于二维差分，我们有：
$$
a[x][y] = \sum_{i = 0}^{x} \sum_{j = 0}^{y} d[i][j]
$$

对于区间查询，我们用前缀和的思路转化为：
$$
\sum_{i = a}^b \sum_{j = c}^d a[i][j] = sum[c][d] - sum[a - 1][d] - sum[b - 1][c] + sum[a - 1][b - 1];
$$

而对于二维前缀和，我们有：
$$
\begin{aligned}
sum[a][b] &= \sum_{i = 0}^{a} \sum_{j = 0}^{b} \sum_{k = 0}^{i} \sum_{l = 0}^{j} c[k][l] \\
&= \sum_{i = 0}^{a} \sum_{j = 0}^b (a - i + 1) \cdot (b - j + 1) \cdot c[i][j] \\
&= \sum_{I = 0}^{a} \sum_{j = 0}^b (ab - a - b + 1) \cdot c[i][j] - 
(b + 1) \cdot i \cdot c[i][j] - (a + 1) \cdot j \cdot c[i][j] + i \cdot j \cdot c[i][j]
\end{aligned}
$$

所以用四个二维树状数组分别维护 $c[i][j]$，$i \cdot c[i][j$，$j \cdot c[i][j]$，$i \cdot j \cdot c[i][j]$ 就可以了。
```c
#include <bits/stdc++.h>

using namespace std;

int n, m, op;

struct BIT {
  vector<vector<long long>> t1, t2, t3, t4;
  
  BIT() = default;
  BIT(int n, int m) {
    t1.assign(n + 1, vector<long long>(m + 1, 0ll));
    t2.assign(n + 1, vector<long long>(m + 1, 0ll));
    t3.assign(n + 1, vector<long long>(m + 1, 0ll));
    t4.assign(n + 1, vector<long long>(m + 1, 0ll));
  }
  
  static int lowbit(int x) {
    return x & (-x);
  }
  
  void add(int x, int y, long long value) {
    for (int i = x; i <= n; i += lowbit(i)) {
      for (int j = y; j <= m; j += lowbit(j)) {
        t1[i][j] += value;
        t2[i][j] += value * x;
        t3[i][j] += value * y;
        t4[i][j] += value * x * y;
      }
    }
  }
  
  long long query(int x, int y) {
    long long retval = 0;
    for (int i = x; i > 0; i -= lowbit(i)) {
      for (int j = y; j > 0; j -= lowbit(j)) {
        retval += (x + 1) * (y + 1) * t1[i][j] - (y + 1) * t2[i][j] - (x + 1) * t3[i][j] + t4[i][j];
      }
    }
    return retval;
  }
} tr;

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  cout.tie(nullptr);
  
  cin >> n >> m;
  
  tr = BIT(n, m);
  
  while (cin >> op) {
    if (op == 1) {
      int a, b, c, d;
      long long value;
      cin >> a >> b >> c >> d >> value;
      
      tr.add(a, b, value);
      tr.add(c + 1, b, -value);
      tr.add(a, d + 1, -value);
      tr.add(c + 1, d + 1, value);
    } else if (op == 2) {
      int a, b, c, d;
      cin >> a >> b >> c >> d;
      
      long long ans = tr.query(c, d) - tr.query(a - 1, d) - tr.query(c, b - 1) + tr.query(a - 1, b - 1);
      
      cout << ans << '\n';
    }
  }
  
  return 0;
}
```