---
title: P10126 「Daily OI Round 3」Pigeon
author: 山暮云秋
description: 人一能之，己百之；人十能之，己千之。
sticky: 0
tags:
  - 数学
categories:
  - 算法题
abbrlink: 21e58d4f
date: 2024-02-08 00:00:00
---

##### 题目描述：[P10126 「Daily OI Round 3」Pigeon](https://www.luogu.com.cn/problem/P10126)

## 解题思路

题目中**原条件等价于**：$\forall i \in [1, n)$，都有 $|a_{i} - y| < |a_{i+1} - y|$。为方便表达，令 $A = a_{i}$，$B = a_{i+1}$

对于上述不等式，根据**绝对值的几何意义**，可理解为在数轴上 $A$ 与 $y$ 的距离比 $B$ 与 $y$ 的距离更近，所以 $y$ 应该在 $A,B$ 中点的**靠近 $A$ 的一侧**，即：

$$\\left\\{ \begin{array}{c} y < \frac{A + B}{2}, A < B \\\\ \\\\ y > \frac{A + B}{2}, A > B \\\\ \end{array} \\right. $$

因此，对于所有 $a_{i} > a_{i+1}$ 的情况(即 $A > B$)，取**中点的最大值** $C_{max}$；对于所有$a_{i} < a_{i+1}$ 的情况(即 $A < B$)，取**中点的最小值** $D_{min}$。则有 $C_{max} < y < D_{min}$

若 $C_{max} \geq D_{min}$ ，则 $y$ **无解**；否则因为有 $D_{min} - C_{max} \geq \frac{1}{2}$，可以取 $y = \frac{C_{max} + D_{min}}{2}$ (以使得$|x_{i+1} - y| - |x_i - y| \geq \frac{1}{4}$，且小数部分有效位数为 $2$，符合题意要求)

注意特判 $a_i = a_{i+1}$ 时，必然**不存在成立的** $y$

## 代码实现

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10;
const double MAX_INF = 2.1e9, MIN_INF = -2.1e9;
int a[N];

void solve()
{
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    double c_max = MIN_INF, d_min = MAX_INF;
    for (int i = 1; i < n; i++)
    {
        // 特判两数相等时，必然不存在，标记最大值
        if (a[i] == a[i + 1])
            c_max = MAX_INF, d_min = MIN_INF;
        else if (a[i] < a[i + 1])
            d_min = min(d_min, double(a[i] + a[i + 1]) / 2);
        else
            c_max = max(c_max, double(a[i] + a[i + 1]) / 2);
    }

    if (c_max >= d_min)
        cout << "pigeon";
    else
        printf("lovely\n%.8lf", (c_max + d_min) / 2);
}

int main()
{
    int T = 1;
    // cin >> T;
    while (T--)
        solve();
    return 0;
}
```

## 页底评论
