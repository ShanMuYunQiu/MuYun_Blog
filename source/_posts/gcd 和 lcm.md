---
title: gcd 和 lcm
author: 圣奇宝枣
description: 学习求 gcd 和 lcm 的方法，学习拓展欧几里得算法
sticky: 0
date: 2023-08-11
updated: 2023-08-11
readmore: true
tags:
  - gcd
  - lcm
  - C++
categories:
  - 算法
---

---

#### **gcd 最大公约数**

---

- **欧几里得算法**

  > 1、求**两个数的最大公约数**，除了**分解质因数**法，我们通常使用**欧几里得算法**(即辗转相除法)，其思路如下  
  > 2、求**m,n 的最大公约数**，就**计算 m / n**，然后**令**`m = n`、`n = m % n`，继续**循环计算**。直到**n = 0**时停止，这时的**m**就是**最大公约数**；或者提前一步，到`m % n == 0`(结果余数为 0)时停止，这时的**n**就是**最大公约数**(推荐后者，在写拓展欧几里得算法时更方便)  
  > 3、**gcd 的性质**：`gcd(a, b) = gcd(a, k*a+b)`；`gcd(k*a, k*b) = k * gcd(a, b)`；`a / gcd(a, b)`**与**`b / gcd(a, b)`**互素**；**多个整数**的**最大公约数**`gcd(a, b, c) = gcd(gcd(a, b), c)`

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

#### **lcm 最小公倍数**

---

- **公式法**

  > 1、求**两个数的最小公倍数**，除了**分解质因数**法，我们通常使用**公式法**，其思路如下  
  > 2、求**m,n 的最小公倍数**，有`gcd(m, n) * lcm(m, n) = m * n`，所以**得到公式**`lcm(m, n) = m * n / gcd(m, n)`

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

#### **拓展欧几里得算法**

---

- **引入**

  > 1、**裴蜀定理**(最大公约数表示定理)：设**整数 a, b** 的**最大公约数**`gcd(a, b)`为 **d**，则必然存在**整数 s, t** 使得`as + bt = d`；特别地，当`gcd(a, b) = 1`(即 a, b 互素)，必然有`as + bt = 1`。在此基础上有**推论**：如果`d | v`(d 是 v 的因子)，那么也有 **s, t** 使得`as + bt = v = kd`；**逆向推导亦然**，如果**存在这样的 s, t**，那么**d 必然是 v 的因子**  
  > 2、与**裴蜀定理**相关的，有**拓展欧几里得算法**，其是用来**计算**`as + bt = gcd(a, b)`中的 **s 和 t** 的。该算法的**本质**是：在执行**欧几里得算法**的过程中，利用每一步计算出来的**余数**和副产品——**商**，顺道通过**迭代公式**计算出每一步的 **s, t**，**最后一步**(实际是倒数第二步)求得的 **s, t**，便是所求的 **s, t**  
  > 3、其中**迭代公式**为：**s<sub>i+1</sub> = s<sub>i-1</sub> - s<sub>i</sub> \* q<sub>i</sub> ， t<sub>i+1</sub> = t<sub>i-1</sub> - t<sub>i</sub> \* q<sub>i</sub>** ，其中 **q 为商**，且 s<sub>0</sub> = 1，s<sub>1</sub> = 0，t<sub>0</sub> = 0，t<sub>1</sub> = 1  
  > 4、举例说明：有`a = 100, b = 35`，则有`100 * s + 35 * t = gcd(100, 35) = 5`，其执行**拓展欧几里得算法**的过程如下表。**算法最后得到**`gcd(100, 35) = 5`，且有 **s = s<sub>3</sub> = -1** ， **t = t<sub>3</sub> = 3**满足**裴蜀定理**

  | 轮次 | 被除数 a | 除数 b | 商 q | 余数 | s<sub>i+1</sub> = s<sub>i-1</sub> - s<sub>i</sub> \* q<sub>i</sub> | t<sub>i+1</sub> = t<sub>i-1</sub> - t<sub>i</sub> \* q<sub>i</sub> |
  | :--: | :------: | :----: | :--: | :--: | :----------------------------------------------------------------: | :----------------------------------------------------------------: |
  |  1   |   100    |   35   |  2   |  30  |                   s<sub>2</sub> = 1 - 0 \* 2 = 1                   |                  t<sub>2</sub> = 0 - 1 \* 2 = -2                   |
  |  2   |    35    |   30   |  1   |  5   |                  s<sub>3</sub> = 0 - 1 \* 1 = -1                   |                 t<sub>3</sub> = 1 - (-2) \* 1 = 3                  |
  |  3   |    30    |   5    |  6   |  0   |                                ---                                 |                                ---                                 |

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

#### **页底评论**

---
