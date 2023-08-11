---
title: gcd 和 lcm
author: 圣奇宝枣
description: 求 gcd 最大公约数，求 lcm 最小公倍数
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
  > 2、求**m,n 的最大公约数**，就**计算 m / n**，然后**令**`m = n`、`n = m % n`，继续**循环计算**，直到**n = 0**时停止，这时的**m**就是**最大公约数**  
  > 3、**gcd 的性质**：`gcd(a, b) = gcd(a, k*a+b)`；`gcd(k*a, k*b) = k * gcd(a, b)`；`a / gcd(a, b)`**与**`b / gcd(a, b)`**互素**；**多个整数**的**最大公约数**`gcd(a, b, c) = gcd(gcd(a, b), c)`

<!-- more -->

- **代码实现**

  ```cpp
  // 循环写法
  int gcd(int m, int n)
  {
      int temp;
      while (n != 0)
      {
          temp = m % n;
          m = n;
          n = temp;
      }
      return m;
  }

  // 递归写法
  int gcd(int m, int n)
  {
      return n == 0 ? m : gcd(n, m % n);
  }
  ```

---

#### **lcm 最小公倍数**

- **公式法**

  > 1、求**两个数的最小公倍数**，除了**分解质因数**法，我们通常使用**公式法**，其思路如下  
  > 2、求**m,n 的最小公倍数**，有`gcd(m, n) * lcm(m, n) = m * n`，所以**得到公式**`lcm(m, n) = m * n / gcd(m, n)`

- **代码实现**

  ```cpp
  // 最大公约数
  int gcd(int m, int n)
  {
      int temp;
      while (n != 0)
      {
          temp = m % n;
          m = n;
          n = temp;
      }
      return m;
  }

  // 最小公倍数
  int lcm(int m, int n)
  {
      return m * n / gcd(m, n);
  }
  ```

---

---

#### **页底评论**

---
