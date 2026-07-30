---
title: '2026 Summer Day13'
description: ''
date: '2026-07-30T05:49:07.431Z'
draft: false
showHeroImage: false
tags: ['数论']
categories: ['记录']
series: ['2026 Summer']
comments: true
sidebar:
  enable: true
  toc: true
  relatedPosts: true
---

# 2026 Summer Day13

## P1495 【模板】中国剩余定理（CRT）/ 曹冲养猪

板子题。

我们求解同余方程组

$$
x \equiv a_i \pmod{m_i}
$$

且 $m_i$ 两两互质的情况下，用如下过程：

+ 令 $M = \prod_i m_i$，

+ 对于每一个 $i$，令 $t_i = \frac{M}{m_i}$。计算 $t_i$ 在模 $m_i$ 下的逆元 $t_i^{-1}$，

+ 令 $c_i = t_it_i^{-1}$，

+ 方程组的解为 $\sum_{i} a_ic_i \pmod{M}$。

 考虑正确性。由于 $t_i = \frac{M}{m_i}$，即 $j \ne i, m_j | t_i$，则对于 $j \ne i$，都有 $t_j \equiv 0 \pmod{n_i}$，所以 $c_j \equiv t_j \equiv 0 \pmod{n_i}$。又因为 $c_i = t_it_i^{-1}$，则 $c_i \equiv 1 \pmod{n_i}$。对于最终答案 $x = \sum_i a_ic_i \pmod{M}$：

$$
x \equiv a_ic_i \equiv a_i \cdot t_i (t_i^{-1} \bmod n_i) \equiv a_i \pmod{n_i}
$$

所以构造的 $x$ 是满足所有同余方程的。

[code](https://www.luogu.com.cn/record/289330633)

## P4777 【模板】扩展中国剩余定理（exCRT）

板子题。

这道题不保证 $m_i$ 两两互质，就不能直接使用上述的构造方式。

我们考虑将每个相邻的同余方程合并。假设我们当前处理的方程为 $x \equiv r_1 \pmod{m_1}$ 和 $x \equiv r_2 \pmod{m_2}$，则 $x$ 可以表示为 $x = k_1m_1 + r_1 = k_2m_2 + r_2$，所以 $k_1m_1 - k_2m_2 = r_2 - r_1$，这是裴蜀定理中 $Ax+By=C$ 的形式，有解需要满足 $\gcd(m_1,m_2) | (r_2-r_1)$ 的条件，这个条件也是同余方程组有解的条件。

考虑这个式子的一般形式 $m_1x - m_2y = \gcd(m_1,m_2)$，我们可以用 exGCD 求解一个特解 $x_0, y_0$，然后 $x_0 \times \frac{r_2-r_1}{\gcd(m_1,m_2)} \to x_0, y_0 \times \frac{r_2-r_1}{\gcd(m_1,m_2)} \to y_0$ 这样我们就得到了方程 $m_1x-m_2y=r_2-r_1$ 的一组解。通解形式为：

$$
\begin{aligned}
x &= x_0 + \frac{m_2}{gcd(m_1, m_2)} \times k \\
y &= y_0 + \frac{m_1}{gcd(m_1, m_2)} \times k
\end{aligned}
$$

再将通解形式带回 $m_1x + r_1$ 中，得到：

$$
\begin{aligned}
x &= m_1 \times (x_0 + \frac{m_2}{\gcd(m_1, m_2)} \times k) + r_1 \\
&= m_1 \times x_0 + lcm(m_1, m_2) \times k + r_1 \\
\end{aligned}
$$

所以 $x \equiv m_1 \times x_0 + r_1 \pmod{lcm(m_1, m_2}$，这样就把两个同余方程合并了。

[code](https://www.luogu.com.cn/record/289346361)

## P3846 【模板】BSGS / [TJOI2007] 可爱的质数

还是板子题。

我们要求离散对数 $x$，满足 $b^x \equiv n \pmod{p}$。

我们将 $x$ 分解为 $i \times \sqrt{p}  - j$ 的形式，则原来的方程可以写为

$$
\begin{aligned}
b^{i \times \sqrt{p} - j} &\equiv n &\pmod p \\
b^{i \times \sqrt{p}} &\equiv n \times b^j &\pmod p
\end{aligned}
$$

这样我们只需要预处理所有 $j < \sqrt{p}$ 的 $n \times b^j$ 并存在一张哈希表里面，然后枚举 $i$ 查表即可，复杂度 $O(\sqrt{p})$。

[code](https://www.luogu.com.cn/record/289414944)

## P3232 [HNOI2013 / JSOI2013] 游走

以这道题复习高斯消元。

如果我们令 $f_i$ 表示节点 $i$ 被走过的期望次数，则：

$$
f_i = \begin{cases}
\sum_{(u, v) \in E, v \ne n} \frac{f_v}{deg(v)} + 1 & i = 1 \\
\sum_{(u, v) \in E, v \ne n} \frac{f_v}{deg(v)} & 1 < i < n
\end{cases}
$$

这显然是一个方程组，可以写成如下形式：

$$
\begin{pmatrix}
1 & \frac{1}{deg(2)} & \frac{1}{deg(3)} & \dots & \frac{1}{deg(n-1)} \\
\frac{1}{deg(1)} & 1 & \frac{1}{deg(3)} & \dots & \frac{1}{deg(n-1)} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
\frac{1}{deg(1)} & \frac{1}{deg(2)} & \frac{1}{deg(3)} & \dots & 1
\end{pmatrix}
\begin{pmatrix}
1 \\ 0 \\ \vdots \\ 0
\end{pmatrix}
$$

然后高斯消元后按照 $f_i$ 的值贪心选取编号即可。

[code](https://www.luogu.com.cn/record/289431492)
