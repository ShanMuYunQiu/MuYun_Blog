---
title: P10126 「Daily OI Round 3」Pigeon
author: 山暮云秋
sticky: 0
readmore: true
tags:
  - 数学
categories:
  - 算法题
date: 2024-02-08 00:00:00
---

##### [P10126 「Daily OI Round 3」Pigeon](https://www.luogu.com.cn/problem/P10126)

<!-- more -->

## 解题思路

1、题目中**原条件等价于**：对于任意`1 <= i < n`，都有`|a[i] - y| < |a[i+1] - y|`。为方便表达，令`a = a[i]`，`b = a[i+1]`  

2、对于上述不等式，根据**绝对值的几何意义**，可理解为在数轴上 **a 与 y 的距离**比 **b 与 y 的距离**更近，所以 y 应该在 **a,b 中点**中**靠近 a 的一侧**。即：**a < b 时**，`y < (a + b) / 2`；**a > b 时**，`y > (a + b) / 2`  

3、因此，对于所有`a[i] > a[i+1]`的情况(即`a > b`)，取**中点的最大值 c_max**；对于所有`a[i] < a[i+1]`的情况(即`a < b`)，取**中点的最小值 d_min**。则`c_max < y < d_min`  

4、若`c_max >= d_min`，则 **y 无解**；否则因为有`d_min - c_max >= 1/2`，可以取`y = (c_max + d_min) / 2`(以使得`|x[i+1] - y| - |x[i] - y| >= 1/4`，且小数部分有效位数为 2，符合要求)

5、注意特判**两数相等时**，必然**不存在成立的 y**

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
