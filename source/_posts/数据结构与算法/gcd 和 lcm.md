---
title: gcd 和 lcm
author: 山暮云秋
description: 学习求 gcd 和 lcm 的方法，学习拓展欧几里得算法、唯一分解定理
sticky: 0
readmore: true
tags:
  - exgcd
  - gcd
  - lcm
  - 数学
  - C++
categories:
  - 数据结构与算法
abbrlink: 394de44a
date: 2023-08-11 00:00:00
updated: 2024-01-21 00:00:00
---

---

### **gcd 最大公约数**

---

- **欧几里得算法**

  > 1、求**两个数的最大公约数**，除了**分解质因数**法，我们通常使用**欧几里得算法**(即辗转相除法)，其思路如下  
  > 2、求 $m, n$ 的**最大公约数**，就**计算** $m \div n$，然后**令** $m = n$、$n = m \bmod n$ ，继续**循环计算**。直到 $n = 0$ 时停止，这时的 $m$ 就是**最大公约数**；或者提前一步，到 $m \bmod n = 0$ (结果余数为 $0$)时停止，这时的 $n$ 就是**最大公约数**(推荐后者，在写拓展欧几里得算法时更方便)  
  > 3、**gcd 的性质**：$gcd(a, b) = gcd(a, ka + b)$ ；$gcd(ka, kb) = k \times gcd(a, b)$ ；$a \div gcd(a, b)$ **与** $b \div gcd(a, b)$ **互素**；**多个整数**的**最大公约数** $gcd(a, b, c) = gcd(gcd(a, b), c)$ ；$a \gt b$ 时，$gcd(a, b) = gcd(a, b-a)$

<!-- more -->

- **代码实现**

  ```cpp
  // 循环写法
  int gcd(int m, int n)
  {
      int temp;
      while (m % n)
      {
          temp = m % n;
          m = n;
          n = temp;
      }
      return n;
  }

  // 递归写法
  int gcd(int m, int n)
  {
      return (m % n) ? gcd(n, m % n) : n;
  }
  ```

---

### **lcm 最小公倍数**

---

- **公式法**

  > 1、求**两个数的最小公倍数**，除了**分解质因数**法，我们通常使用**公式法**，其思路如下  
  > 2、求 $m,n$ 的**最小公倍数**，有 $gcd(m, n) \times lcm(m, n) = m \times n$，所以**得到公式** $lcm(m, n) = m \times n \div gcd(m, n)$

- **代码实现**

  ```cpp
  // 最大公约数
  int gcd(int m, int n)
  {
      int temp;
      while (m % n)
      {
          temp = m % n;
          m = n;
          n = temp;
      }
      return n;
  }

  // 最小公倍数
  int lcm(int m, int n)
  {
      return m * n / gcd(m, n);
  }
  ```

---

### **拓展欧几里得算法**

---

- **拓展欧几里得算法**

  > 1、**裴蜀定理**(最大公约数表示定理)：设$a, b \in Z$ 的**最大公约数** $gcd(a, b) = d$，则必然存在 $s, t \in Z$ 使得 $as + bt = d$ ；特别地，当 $gcd(a, b) = 1$ (即 $a, b$ 互素)，必然有 $as + bt = 1$ 。在此基础上有**推论**：如果 $d | v$ ($d$ 是 $v$ 的因子)，那么也有 $s, t$ 使得 $as + bt = v = kd$ ；**逆向推导亦然**，如果**存在这样的** $s, t$，那么 $d$ 必然是 $v$ 的**因子**  
  > 2、与**裴蜀定理**相关的，有**拓展欧几里得算法**，其是用来**计算** $as + bt = gcd(a, b)$ 中的 $s, t$ 的。该算法的**本质**是：在执行**欧几里得算法**的过程中，利用每一步计算出来的**余数**和副产品——**商**，顺道通过**迭代公式**计算出每一步的 $s, t$ ，**最后一步**(实际是倒数第二步)求得的 $s, t$ ，便是所求的 $s, t$  
  > 3、其中**迭代公式**为：$s_{i+1} = s_{i-1} - s_i \times q_i$ ，$t_{i+1} = t_{i-1} - t_i \times q_i$ 。其中 $q$ 为**商**，且 $s_0 = 1, s_1 = 0, t_0 = 0, t_1 = 1$  
  > 4、举例说明：有 $a = 100, b = 35$ ，则有 $100s + 35t = gcd(100, 35) = 5$ ，其执行**拓展欧几里得算法**的过程如下表。**算法最后得到** $gcd(100, 35) = 5$ ，且有 $s = s_3 = -1$， $t = t_3 = 3$ 满足**裴蜀定理**

  | 轮次 | 被除数 a | 除数 b | 商 q | 余数 | $s_{i+1} = s_{i-1} - s_i \times q_i$ | $t_{i+1} = t{i-1} - t_i \times q_i$ |
  | :--: | :------: | :----: | :--: | :--: | :----------------------------------: | :---------------------------------: |
  |  1   |   100    |   35   |  2   |  30  |      $s_2 = 1 - 0 \times 2 = 1$      |     $t_2 = 0 - 1 \times 2 = -2$     |
  |  2   |    35    |   30   |  1   |  5   |     $s_3 = 0 - 1 \times 1 = -1$      |    $t_3 = 1 - (-2) \times 1 = 3$    |
  |  3   |    30    |   5    |  6   |  0   |                 ---                  |                 ---                 |

- **代码实现**

  ```cpp
  #include <cstdio>
  #include <iostream>
  using namespace std;

  // 循环写法
  int ex_gcd(int a, int b, int &s, int &t)
  {
      // s, t 表示 s_i 和 t_i，其默认值为 s_1 = 0 和 t_1 = 1
      // s_ 和 t_ 表示 s_i-1 和 t_i-1，其默认值表示 s_0 = 1 和 t_0 = 0
      int s_ = 1, t_ = 0, q, temp;
      while (a % b)
      {
          q = a / b;

          // 迭代公式：s_i+1 = s_i-1 - s_i * q_i
          temp = s;       // 记录当前 s_i
          s = s_ - s * q; // 计算 s_i+1
          s_ = temp;      // 下一轮 s_i
          // 迭代公式计算 t_i+1
          temp = t;
          t = t_ - t * q;
          t_ = temp;

          temp = a % b;
          a = b;
          b = temp;
      }
      return b;
  }

  // 递归写法
  int ex_gcd(int a, int b, int &s, int &t)
  {
      if (b == 0)
      {
          s = 1, t = 0;
          return a;
      }

      int gcd = ex_gcd(b, a % b, t, s);
      t -= (a / b) * s;
      return gcd;
  }

  int main()
  {
      // s, t 表示 s_i 和 t_i，其默认值为 s_1 = 0 和 t_1 = 1
      int a, b, s = 0, t = 1;
      cin >> a >> b;
      int gcd = ex_gcd(a, b, s, t);
      printf("%d(a) * %d(s) + %d(b) * %d(t) = %d(gcd(a, b))", a, s, b, t, gcd);
      return 0;
  }
  ```

---

### **唯一分解定理**

---

- **唯一分解定理**

  > 1、**内容**：任意一个**大于 $1$ 的自然数** $N$，都可以表示为**有限个质数的乘积**。即：$N = P_1^{a_1} \times P_2^{a_2} \times P_3^{a_3} \times \ldots$ ，其中 $P_i$ 为**质数**，$a_i$ 为**指数**。例如 $12 = 2^2 \times 3^1$  
  > 2、**有关正因数个数**：对于**大于 $1$ 的自然数** $N$，其**正因数的个数**为：$ct = (1 + a_1) \times (1 + a_2) \times (1 + a_3) \times \ldots \times (1 + a_n)$  
  > 3、**有关 gcd 和 lcm**：通过**唯一分解定理**可表示 $gcd(a, b) = P_1^{min(a_1, b_1)} \times P_2^{min(a_2, b_2)} \times P_3^{min(a_3, b_3)} \times \ldots$ ；$lcm(a, b) = P_1^{max(a_1, b_1)} \times P_2^{max(a_2, b_2)} \times P_3^{max(a_3, b_3)} \times \ldots$

- **区间 lcm**

  > 1、[区间 lcm](https://www.starrycoding.com/problem/116)：给定 $l$ 和 $r$ ($l,r \leq 40000$)，输出 $[l, r]$ **区间内所有数**的**最小公倍数**，结果对 $10^9 + 7$ 取模  
  > 2、由上结论得：解决此问题，只需要通过**唯一分解定理**，对 $[l, r]$ 间的数**分解质因数**，并分别记录所有**质因子**的**最大指数**，最后再**将所有质因子相乘**  
  > 3、代码实现时，要先通过**质数筛**筛选**范围内的所有质数**；再依次**分解质因数**，分别记录**质因子**的**最大指数**，注意最后需要**特判**；为优化最后**质因子相乘**的效率，通过**快速幂**实现相乘  
  > 4、同理可得，对于**区间 gcd**，只需要记录**所有质因子**的**最小指数**，但记录的数组需要提前**初始化为无穷**，并在最后**相乘时判断**只相乘**出现过的质因子**(数量不为无穷的质因子)

- **代码实现**

  ```cpp
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 4e4 + 10;
  const int P = 1e9 + 7;

  vector<int> prime;
  bitset<N> not_prime;
  int cnt[N]; // 表示 lcm 的指数

  // 快速幂取模
  long long ModExp(long long a, long long b)
  {
      long long res = 1;
      while (b)
      {
          if (b & 1)
              res = res * a % P;
          a = a * a % P;
          b >>= 1;
      }
      return res;
  }

  // 欧拉线性筛
  void oler(int n)
  {
      for (int i = 2; i <= n; i++)
      {
          if (!not_prime[i])
              prime.push_back(i);

          for (int j = 0; prime[j] * i <= n; j++)
          {
              not_prime[prime[j] * i] = true;
              if (i % prime[j] == 0)
                  break;
          }
      }
  }

  // 唯一分解定理：质因数分解
  void func(int n)
  {
      for (int i = 2; i <= n / i; i++)
      {
          // 如果 n 不能整除 i，说明 i 不是 n 的质因子
          if (n % i)
              continue;

          int tmp = 0;  // 记录质因子的指数
          while (n % i == 0)
          {
              n /= i;
              tmp++;
          }
          cnt[i] = max(cnt[i], tmp);  // 记录质因子 i 的最大指数
      }

      // 特判 n 为质数(此时 n > 1)的情况
      if (n > 1)
          cnt[n] = max(cnt[n], 1);
  }

  void solve()
  {
      oler(4e4);

      int l, r;
      cin >> l >> r;
      for (int i = l; i <= r; i++)
          func(i);

      long long ans = 1;
      // 相乘所有质因子，得到 lcm
      for (int i = 2; i <= r; i++)
      {
          if (!not_prime[i])
              ans = ans * ModExp(i, cnt[i]) % P;
      }
      cout << ans;
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
