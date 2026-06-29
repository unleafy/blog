---
title: '2025 Summer Day10'
description: '暑期集训 Day10'
date: '2025-07-24T03:22:39.534Z'
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


# 2025 Summer Day10


**Content**：dp优化

**Date**：2025.7.26



# 例题

## [洛谷-P1886](www.luogu.com.cn/problem/P1886) 滑动窗口

### 思路

直接单调队列维护即可，具体操作如下：

1. 每次查看队尾的元素，维护单调性。
2. 对于队头不在当前滑动窗口的元素，弹出。

提交记录：[link](https://www.luogu.com.cn/record/226955517)

## [洛谷-P2365](www.luogu.com.cn/problem/P2365) 任务安排

### 思路

定义 $dp_{i}$ 表示第 $i$ 个任务完成所花费的最小代价。转移如下：

$$
dp_{i} = \min_{1 \le j \le i} dp_{j} + (s + T_i - T_{j-1}) \times (F_{n} - F{j - 1})
$$

对于本题数据范围 $n \le 3000$，$O(n^2)$ 可过。

提交记录：[link](https://www.luogu.com.cn/record/226972772)

## [洛谷-P10979](www.luogu.com.cn/problem/P10979) 任务安排 2

### 思路

$dp$ 的方式和定义与前面相同，但是 $O(n^2)$ 的复杂度无法接受，所以考虑优化。

将 $dp$ 的式子拆开，发现：

$$
dp_{i} = \min_{1 \le j \le i} dp_{j} + F_{i}T_{i} - F_{j}T_{i} + F_{n}s - F_{j}s
$$

移项，得：

$$
    dp_{j} = (T_{i} + s)F_{j} + (dp_{i} - F_{i}T_{i} - F_{n}s)
$$

很明显可以用斜率优化，单调队列维护下凸壳即可。

提交记录：[link](https://www.luogu.com.cn/record/227051418)

## [洛谷-P5785](www.luogu.com.cn/problem/P5785) [SDOI2012] 任务安排

### 思路

注意数据范围，$\lvert T_i \rvert \le 2^8$，所以 $T_{i}+s$ 不具有单调性，所以上面面直接维护的方法不可做。于是我们考虑直接维护整个下凸壳，对于转移时，二分查找就可以了。

提交记录：[link](https://www.luogu.com.cn/record/227053295)

## [洛谷-P4072](www.luogu.com.cn/problem/P4072) [SDOI2016] 征途

### 思路

我们定义 $dp_{i,j}$ 表示到第 $i$ 天走到了第 $j$ 个城市的最小平方和。之所以这样定义，是因为：

$$
\begin{aligned}
s^2 \times m^2 &= \frac{\sum_{i} (s_i - \overline{s})^2}{m} \times m^2 \\
&= \frac{(\frac{\sum_i s_i}{m})^2 - 2 \times \frac{\sum_i s_i}{m} \times \sum_i si + \sum_i s_i^2}{m} \times m^2 \\
&= -(\sum_i s_i)^2 + m \times \sum_i s_i^2
\end{aligned}
$$

所以最后的答案只和后面的平方和有关。

转移还是单调队列维护下凸壳即可。

提交记录：[link](https://www.luogu.com.cn/record/227068221)