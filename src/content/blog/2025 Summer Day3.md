---
title: '2025 Summer Day3'
description: '暑期集训 Day3'
date: '2025-07-17T03:22:39.534Z'
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

# 2025 Summer Day3


**Content**： Data structs

**Date**：2025.7.19



## 内容

+ 三维偏序问题
+ CDQ分治
+ 整体二分
+ 分块
+ 莫队算法

## 具体内容

### 三维偏序问题


#### 问题描述

>给定一些三元组 $(a_i, b_i, c_i)$，询问对于三元组 $(a_j, b_j, c_j)$，有多少个三元组满足 $a_i \le a_j$ 且 $b_i \le b_j$ 且 $c_i \le c_j$。


首先对于这个问题，我们考虑先去重，然后排序。排序规则如下：
  + 如果 $a_i \ne a_j$，则返回 $a_i < a_j$
  + 如果 $b_i \ne b_j$，则返回 $b_i < b_j$
  + 否则返回 $c_i < c_j$

这样我们就把 $a_i < a_j$ 的条件去掉了。

接下来我们考虑分治，定义函数 $solve(l, r)$ 表示当前解决到了区间 $[l, r]$，取其中点 $mid$，将原序列的问题转化为三个部分：
+ $j < i \le mid$，由 $solve(l, mid)$ 解决。
+ $mid < j < i$，由 $solve(mid + 1, r)$ 解决。
+ $j \le mid < i$，即被 $mid$ 分成了两个部分。

对于第三种情况，我们需要解决的是如下问题：


#### 问题描述

>在区间 $[l,r]$ 中，有多少对二元组 $(i, j)$ （其中 $i \ne j$）满足 $b_i < b_j$ 且 $c_i < c_j$。



可见问题转化为了二维偏序问题，用线段树解决就可以。

### 整体二分

普通的二分操作是直接对于每个询问二分答案，判断答案是否合法。
整体二分的思路是将所有询问一起二分，根据二分的答案对询问进行分类，再逐步求解。

#### [洛谷 P3332](https://www.luogu.com.cn/problem/P3332) 题目描述

你需要维护 n 个可重整数集，集合的编号从 1 到 n。  
这些集合初始都是空集，有 m 个操作：

 - `1 l r c`：表示将 c 加入到编号在 $[l,r]$ 内的集合中
 - `2 l r c`：表示查询编号在 $[l,r]$ 内的集合的并集中，第 c 大的数是多少。

 注意可重集的并是不去除重复元素的，如 $\set{1,1,4} \cup \set{5,1,4}=\set{1,1,4,5,1,4}$。

我们还是定义 $solve(l, r, ql, qr)$ 表示当前答案区间为 $[l, r]$，处理的操作区间为 $[ql, qr]$，取答案中点 $mid$。将操作分为如下四类

+ 如果当前操作是修改 （$op = 1$）
  + 如果 $c \le mid$：归为 $A$ 类，由左递归处理。
+ 如果 $c > mid$：归为 $B$ 类，由右递归处理，且用树状数组维护当前比 $mid$ 大的数的个数（差分）。
+ 如果当前操作是查询（$op = 2$）
  + 如果 $c \le mid$：归为 $A$ 类，由左递归处理。
  + 如果 $c > mid$：归为 $B$ 类，由右递归处理，并调整 $c$ 的值，以保证结果正确性。

```c
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6 + 5;
struct Node {
  int l, r, k;
} q[N];
int pos[N], cnt[N], a[N], ans = 0, rec[N];
int n, m;

bool cmp(Node a, Node b) {
  if (pos[a.l] != pos[b.l]) return pos[a.l] < pos[b.l];
  if (pos[a.l] & 1) return a.r > b.r;
  return a.r < b.r;
}

void add(int x) {
  cnt[a[x]]++;
  if (cnt[a[x]] == 1) ans++;
}

void del(int x) {
  cnt[a[x]]--;
  if (cnt[a[x]] == 0) ans--;
}

int main() {
  cin >> n;

  int block = sqrt(n);

  for (int i = 1; i <= n; i++) {
    cin >> a[i];
    pos[i] = (i - 1) / block + 1;
  }

  cin >> m;

  for (int i = 1; i <= m; i++) {
    cin >> q[i].l >> q[i].r;
    q[i].k = i;
  }

  sort(q + 1, q + m + 1, cmp);

  int L = 1, R = 0;

  for (int i = 1; i <= m; i++) {
    while (L < q[i].l) {
      del(L);
      L++;
    }
    while (R > q[i].r) {
      del(R);
      R--;
    }
    while (L > q[i].l) {
      L--;
      add(L);
    }
    while (R < q[i].r) {
      R++;
      add(R);
    }

    rec[q[i].k] = ans;
  }

  for (int i = 1; i <= m; i++) cout << rec[i] << endl;

  return 0;
}
```

### 莫队算法

#### 莫队

莫队算法即为优美的暴力，根据分块对询问排序，然后维护双指针统计答案。

```c
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6 + 5;
struct Node { 
	int l, r, k;
}q[N];
int pos[N], cnt[N], a[N], ans = 0, rec[N];
int n, m;

bool cmp(Node a, Node b) {
	if(pos[a.l] != pos[b.l]) return pos[a.l] < pos[b.l];
	if(pos[a.l] & 1) return a.r > b.r;
	return a.r < b.r;
}

void add(int x) {
	cnt[a[x]] ++;
	if(cnt[a[x]] == 1) ans ++;
}

void del(int x) {
	cnt[a[x]] --;
	if(cnt[a[x]] == 0) ans --;
}

int main() {
	cin >> n;
	
	int block = sqrt(n);
	
	for(int i = 1; i <= n; i ++) {
		cin >> a[i];
		pos[i] = (i - 1) / block + 1;
	}
	
	cin >> m;
	
	for(int i = 1; i <= m; i ++) {
		cin >> q[i].l >> q[i].r;
		q[i].k = i;
	}
	
	sort(q + 1, q + m + 1, cmp);
	
	int L = 1, R = 0;
	
	for(int i = 1; i <= m; i ++) {
		while(L < q[i].l) {
			del(L);
			L ++;
		}
		while(R > q[i].r) {
			del(R);
			R --;
		}
		while(L > q[i].l) {
			L --;
			add(L);
		}
		while(R < q[i].r) {
			R ++;
			add(R);
		}
		
		rec[q[i].k] = ans;
	}
	
	for(int i = 1; i <= m; i ++) cout << rec[i] << endl;
	
	return 0;
}
```

#### 带修莫队

在普通莫队的基础上维护修改操作的时间戳 $t$，每次移动左右端点时同时维护修改操作，根据询问的时间戳同步修改/撤销操作。

[洛谷 P1903](https://www.luogu.com.cn/problem/P1903)

```c
#include <bits/stdc++.h>

using namespace std;

const int N = 1e6 + 5;
int a[N], ans[N], pos[N];
int L[N], R[N];
int cnt[N];

struct query {
  int L, R, time, id;
} ask[N];

struct modify {
  int pos, color, last;
} c[N];

int cntq, cntc, n, m, block, num;
int times, now;

bool cmp(query a, query b) {
  bool ret = false;

  if (pos[a.L] ^ pos[b.L]) {
    ret = pos[a.L] < pos[b.L];
  } else if (pos[a.R] ^ pos[b.R]) {
    ret = pos[a.R] < pos[b.R];
  } else {
    ret = a.time < b.time;
  }

  return ret;
}

void add(int x) {
  int val = a[x];
  if (!cnt[val]) now++;
  cnt[val]++;
}

void del(int x) {
  int val = a[x];
  cnt[val]--;
  if (!cnt[val]) now--;
}

void change() {
  int P = c[times].pos;
  cnt[a[P]]--;
  if (!cnt[a[P]]) now--;

  int COL = c[times].color;
  if (!cnt[COL]) now++;
  cnt[COL]++;
}

int main() {
  cin >> n >> m;

  block = pow(n, 2.0 / 3.0);

  num = ceil((double)n / block);

  for (int i = 1; i <= num; i++) {
    L[i] = (i - 1) * block + 1;
    R[i] = i * block;
  }

  for (int i = 1; i <= num; i++) {
    for (int j = L[i]; j <= R[i]; j++) {
      pos[j] = i;
    }
  }

  for (int i = 1; i <= n; i++) {
    cin >> a[i];
  }

  for (int i = 1; i <= m; i++) {
    string opt;
    cin >> opt;

    if (opt[0] == 'Q') {
      cntq++;

      cin >> ask[cntq].L >> ask[cntq].R;

      ask[cntq].time = cntc;
      ask[cntq].id = cntq;
    } else if (opt[0] == 'R') {
      cntc++;

      cin >> c[cntc].pos >> c[cntc].color;
    }
  }

  sort(ask + 1, ask + cntq + 1, cmp);

  int left = 1, right = 0;
  times = 0;
  now = 0;

  for (int i = 1; i <= cntq; i++) {
    int ql = ask[i].L, qr = ask[i].R, qt = ask[i].time;

    while (left < ql) del(left++);
    while (left > ql) add(--left);
    while (right < qr) add(++right);
    while (right > qr) del(right--);

    while (times < qt) {
      times++;
      if (ql <= c[times].pos && c[times].pos <= qr) {
        change();
      }

      swap(a[c[times].pos], c[times].color);
    }

    while (times > qt) {
      if (ql <= c[times].pos && c[times].pos <= qr) {
        change();
      }
      swap(a[c[times].pos], c[times].color);
      times--;
    }

    ans[ask[i].id] = now;
  }

  for (int i = 1; i <= cntq; i++) {
    cout << ans[i] << endl;
  }

  return 0;
}
```

#### 回滚莫队

即每次操作后都将左指针回退到分块的左端点，然后再进行下一次操作。