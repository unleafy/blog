---
title: '2025 Summer Day28'
description: '暑期集训 Day28'
date: '2025-08-09T03:22:39.534Z'
draft: false
tags:
  - 图论
  - 网络流
series:
 - '2025 Summer'
showHeroImage: false
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2025 Summer Day28


**Content**：网络流进阶
**Date**：2025.8.13


~~昨天太累了，没写，就今天补上吧~~

# 课堂内容

## 上下界网络流

OI Wiki：[Link](https://oi-wiki.org/graph/flow/bound/)

很可惜，没学懂，但是大概意思应该是用差分的思想将原来的上下界网络流转换为网络最大流。~~没学过真的听不懂啊……~~

# 题目

## 模拟题-6.2 异或

### 题目大意

给你一个长度为 $n$ 的数组 $a$，问你满足 $(a_{i} \oplus a_{j}) < (a_{j} \oplus a_{k})$ 的三元组 $(i,j,k)$ 有多少个。

### 解题思路

由于异或 **相同为 0，不同为 1** 的运算规则，可以发现要满足上述的不等式 $(a_{i} \oplus a_{j}) < (a_{j} \oplus a_{k})$ 只与这三个数第一个在二进制下不同的位置 $p$ 有关。具体来说：

+ 若 $a_{k,p} = 1$，则 $a_{i,p} = a_{j,p} = 0$;
+ 若 $a_{k,p} = 0$，则 $a_{i,p} = a_{j,p} = 1$。

发现其实和二进制串的匹配有关，可以用 0/1 字典树解决，在字典树上计算贡献。

### Code

```c
#include <bits/stdc++.h>

using namespace std;

constexpr int N = 5e5 + 5;
constexpr int Z = 2;
long long ans = 0;
int n, arr[N], mark[50][2];

struct Trie {
    int tr[N * 32][Z], cnt = 1;
    long long pass[N * 32][2];
    
    Trie() {
        // memset(tr, -1, sizeof(tr));
        // memset(pass, 0, sizeof(pass));
        tr[0][0] = tr[0][1] = -1;
        cnt = 1;
    }
    
    void Insert(int number) {
        int pos = 0;
        pass[pos][1]++;
        
        for (int i = 30; i >= 0; i--) {
            int ch = (number >> i) & 1;
            
            if (tr[pos][ch] == -1) {
                tr[cnt][0] = tr[cnt][1] = -1;
                pass[cnt][0] = pass[cnt][1] = 0;
                tr[pos][ch] = cnt++;
            }
            
            pos = tr[pos][ch];
            pass[pos][0] += mark[i][ch];
            pass[pos][1]++;
            mark[i][ch]++;
            
            // cout << "Insert: " << pos << ' ' << pass[pos][0] << ' ' << pass[pos][1] << ' ' << mark[i][ch] << '\n';
        }
    }
    
    long long Query(int number) {
        int pos = 0;
        long long retval = 0;
        
        for (int i = 30; i >= 0; i--) {
            int ch = (number >> i) & 1;
            int rev = 1 - ch;
            
            // ! 
            if (tr[pos][rev] != -1) {
                // cout << pass[tr[pos][rev]][0] << ' ' << pass[tr[pos][rev]][1] << " " << mark[i][rev] << "\n";
                
                retval += 1ll * pass[tr[pos][rev]][1] * mark[i][rev] - pass[tr[pos][rev]][1] - pass[tr[pos][rev]][0];
            }
                
            pos = tr[pos][ch];
            if (pos == -1) {
                break;
            }
        }
        
        return retval;
    }
} t;

int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    
    memset(mark, 0, sizeof(mark));
    
    cin >> n;
    
    for (int i = 1; i <= n; i++) {
        cin >> arr[i];
    }
    
    for (int i = 1; i <= n; i++) {
        ans += t.Query(arr[i]);
        
        // cout << "\n";
        
        t.Insert(arr[i]);
    }
    
    cout << ans << '\n';
    
    return 0;
}
```