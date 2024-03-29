---
title: E. Final Countdown
author: 山暮云秋
sticky: 0
readmore: true
tags:
  - 前缀和
  - 高精度
categories:
  - 算法题
abbrlink: 8abc54c0
date: 2024-02-19 00:00:00
---

##### [E. Final Countdown](https://codeforces.com/contest/1932/problem/E)

<!-- more -->

## 解题思路

1、题目大意：给一个数字(如 12345)，每次**将其减 1**，**每次的代价**为**发生变化的数位个数**，如 12340 发生变化变为 12339 时，**代价为 2**。求出**数字变为 0 之前的总代价**  

2、分析可知，以 12345 为例：其个位变化了 12345 次，十位变化了 1234 次，百位变化了 123 次……所以总代价为`12345 + 1234 + 123 + 12 + 1 = 13715`

3、但题目的数据范围较大，直接相加会**超出数据范围**。此时可能会想到用**高精度**，但高精度**每次计算都是 O(n)**，又有 **n 次运算**，复杂度达到 **O(n^2)** 会**超时**  

4、找到规律：如果将要加的数**按照竖式右对齐**列出，则不难发现答案中，`个位 = 1 + 2 + 3 + 4 + 5`，`十位 = 1 + 2 + 3 + 4`……显然，如果**将 n 从高位向低位编号**，**答案的第 i 位** = **n 从第一位到第 i 位**的**各数位数字之和**，这样的**区间和**可以使用**前缀和**`sum[1, i]`优化。

5、再类比**高精度**的思想，**模 10 保留第 i 位的结果**，**除以 10 向上一位进位**，其余按照**高精度**的方式操作即可

## 代码实现

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 4e5 + 10;

int prefix[N]; // 即 sum[1, i]

void solve()
{
    int n;
    cin >> n;
    char s[N]; // 存储数字
    for (int i = 1; i <= n; i++)
        cin >> s[i];

    // 处理前缀和
    prefix[0] = 0;
    for (int i = 1; i <= n; i++)
        prefix[i] = prefix[i - 1] + s[i] - '0'; // 统计从第 1 位到第 i 位的各数位数字之和

    // 类比高精度，处理进位和数位
    for (int i = n; i >= 1; i--)
    {
        prefix[i - 1] += prefix[i] / 10;
        prefix[i] %= 10;
    }

    // 输出答案
    bool flag = false;
    for (int i = 0; i <= n; i++)
    {
        if (prefix[i] || flag)
        {
            flag = true;
            cout << prefix[i];
        }
    }
    cout << '\n';
}

int main()
{
    ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
    int T = 1;
    cin >> T;
    while (T--)
        solve();
    return 0;
}
```

## 页底评论
