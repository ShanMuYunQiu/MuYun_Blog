---
title: D - K-th Nearest
author: 山暮云秋
sticky: 0
tags:
  - 二分查找
  - 二分答案
categories:
  - 算法题
abbrlink: 8e48091a
date: 2024-07-29 00:00:00
---

##### 题目描述：[D - K-th Nearest](https://atcoder.jp/contests/abc364/tasks/abc364_d)

---

### **解题思路**

---

- **解题思路**

  > 1、对于该题，显然首先要使 $a[]$ **升序排列**。对于**每一个询问的** $b, k$，即从 $b$ 点开始**向两侧查找**第 $k$ 个遇到的 $a[]$ 中的点，求它们的距离。可以换一种方法表述  
  > 2、若以 $b$ 为**圆心**，**向外画圆**使得圆内**恰好包含** $k$ **个点**，此时的**半径**即为答案。特殊情况是，若恰好**扩大半径**时**同时包含了两个新的点**，答案仍然成立。
  > 3、即，要找到一个 $r_{min}$，使得**区间** $[b - r, b + r]$ 包含**至少** $k$ **个点**。又因为**区间** $[b - r, b + r]$ 包含的**点的个数**一定是**单调递增**的，所以可以用**二分答案**将**最值问题**转换为**判断问题**求解 $r$  
  > 4、判断一个区间内**包含多少个点**，由于这些**点的坐标**也是**单调递增**的，所以可以**二分查找**。由于$a[]$存储的是**点的坐标**，在**区间** $[L, R]$ 中，找到首个 $a[i] \geq L$，$a[j] \gt R$ (左侧第一个点，右侧出界第一个点)，则区间内**点的个数**为**点序** $j - i$。代码中`lower_bound`和`upper_bound`返回的**指针相减**效果相同

---

### **代码实现**

---

- **代码实现**

  ```cpp
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 1e5 + 5;
  int a[N];
  int n, q;

  // 返回区间 [L, R] 中有多少个点
  int cnt(int L, int R)
  {
      // 第一个 > R 的点序，减去第一个 >= L 的点序 (此处指针相减)
      return upper_bound(a + 1, a + 1 + n, R) - lower_bound(a + 1, a + 1 + n, L);
  }

  void solve()
  {
      cin >> n >> q;
      for (int i = 1; i <= n; i++)
          cin >> a[i];
      sort(a + 1, a + 1 + n); // 排序

      // 开始询问
      while (q--)
      {
          int b, k;
          cin >> b >> k;

          // 二分答案，求 r_min 使得 cnt[b - r, b + r] >= k
          int L = -1, R = 1e9, mid;
          while (L + 1 != R)
          {
              mid = (L + R) / 2;
              // mid 是可行的，则答案应 <= mid，使 R = mid
              if (cnt(b - mid, b + mid) >= k)
                  R = mid;
              else
                  L = mid;
          }
          cout << R << '\n';
      }
  }

  int main()
  {
      ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
      int T = 1;
      // cin >> T;
      while (T--)
          solve();
      return 0;
  }
  ```

---

### **页底评论**

---
