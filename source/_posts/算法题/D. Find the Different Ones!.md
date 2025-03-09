---
title: D. Find the Different Ones!
author: 山暮云秋
sticky: 0
tags:
  - 前缀和
  - 数学
categories:
  - 算法题
abbrlink: 86fe45f1
date: 2024-02-09 00:00:00
---

##### 题目描述：[D. Find the Different Ones!](https://codeforces.com/contest/1927/problem/D)

---

### **解题思路**

---

- **解题思路**

  > 1、假设一开始数列中**所有数**都是**相同的**，此时是无解的，研究**出现不同**时的特点。此时如果有一个 $a_x$ 的值**与其他元素不同**，则一定有 $a_{x-1} \\not= a_x$ (相邻位置元素不同)  
  > 2、一种方法是 **set** 加**二分**：遍历序列，当出现 $a_x \\not= a_{x-1}$ 时，将 $\\{ x-1,x \\}$ 计入 **set**。**询问区间** $[L, R]$ 时，通过**二分**从 **set** 中查找一个大于 $L$ 的 $x$，这一组 $\\{ x-1,x \\}$ 便**可以是答案**  
  > 3、另一种方法是**前缀和**加**二分**：用**前缀和**的方式处理一个 $s[]$，令 $s[i]$ 表示区间 $[1, i]$ **共有多少个** $a_{x-1} \\not= a_x$，则 $s[x] - s[L]$ 便可表示区间 $[L, x]$ 的情况，只要 $s[x] - s[L] > 0$ ，则一定有**满足题意**的结果。简单可知，$s[]$ 一定是**单调不减**的(升序)，所以可以用**二分**的方式，找到一个大于 $s[L]$ 的 $s[x]$ ，则 $\\{x-1, x\\}$ 便**可以是答案**  
  > 4、下面的代码实现，采用的是**第二种方法**

---

### **代码实现**

---

- **代码实现**

  ```cpp
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 2e5 + 10;

  int a[N], s[N];

  void solve()
  {
      int n;
      cin >> n;
      for (int i = 1; i <= n; i++)
          cin >> a[i];

      // 处理 s[]
      for (int i = 2; i <= n; i++)
          s[i] = s[i - 1] + (a[i] != a[i - 1]);

      int q;
      cin >> q;
      while (q--)
      {
          int l, r;
          cin >> l >> r;

          // 二分查找一个 > s[l] 的 s[x]
          int x = upper_bound(s + l + 1, s + r + 1, s[l]) - s;
          if (x > r)
              cout << "-1 -1\n";
          else
              cout << x - 1 << ' ' << x << '\n';
      }
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

---

### **页底评论**

---
