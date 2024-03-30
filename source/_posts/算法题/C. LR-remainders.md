---
title: C. LR-remainders
author: 山暮云秋
sticky: 0
readmore: true
tags:
  - 模拟
categories:
  - 算法题
abbrlink: 5c63dfdb
date: 2024-02-19 00:00:00
---

##### [C. LR-remainders](https://codeforces.com/contest/1932/problem/C)

<!-- more -->

## 解题思路

该题有很多种**思维误区**，如下：

**误区 1**：预先将所有数字相乘会**超出数据范围**，可能会使用**高精度**，但由于**运算操作**占据复杂度，会**超时**

**误区 2**：如果通过**模运算乘法**预处理**整体**，每次**去掉一个数**可以看作**整体**在**模意义**下**除以该数**，可能会通过**乘法逆元**转换成**模乘法**，但**题目条件**并不能满足**求逆元**的要求(不能保证 a, m 互素，或 m 一定为质数的前提条件)

事实上，不妨**逆向来想**，既然知道**删除顺序**，那就可以反向模拟**用乘法还原**：按照**删除顺序**，依次记录下按照顺序删除的元素是什么(参照题目第 3 组样例，其存储`{6, 1, 2, 3, 5, 4}`)，这里可以用**栈**存储(方便反向还原时使用，此时栈顶为最后一个删除的元素，栈第二个元素为倒数第二个删除的元素)

从栈顶依次**取出删除的元素**，通过**模乘法**模拟还原回去。此时**还原栈顶元素**得到的答案即为**输出的最后一个答案**，还原**栈第二个元素**得到的答案即为**输出的倒数第二个答案**。同样**用栈存储答案**，这样便**逆序输出答案**

## 代码实现

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;
int a[N];

void solve()
{
    int n, m;
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> a[i];

    stack<int> stk; // 按删除顺序存储删除的元素
    int L = 1, R = n;
    while (n--)
    {
        char opt;
        cin >> opt;

        if (opt == 'L')
            stk.push(a[L++]);
        else
            stk.push(a[R--]);
    }

    stack<int> ans; // 存储答案
    int now = 1;
    while (stk.size())
    {
        int x = stk.top();
        stk.pop();

        now = now * x % m; // 用模乘法还原
        ans.push(now);
    }

    while (ans.size())
    {
        cout << ans.top() << ' ';
        ans.pop();
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
