---
title: 01Trie
author: 山暮云秋
description: 学习 01Trie 字典树解决异或对问题，求最大异或对、第 k 小异或对
sticky: 0
readmore: true
tags:
  - 01Trie
  - 树
  - C++
categories:
  - 数据结构与算法
abbrlink: b30456ca
date: 2024-08-11 00:00:00
updated: 2024-08-11 00:00:00
---

---

### **01Trie-最大异或对**

---

- **01Trie**

  > 1、**Trie** 指**字典树**：简单地说，对于 $abcd, abcc, abca, acca$ 这四个**字符串**，互相之间有**相同前缀**，将它拆分成字符，建成**边权**为**字符**、**点权**为到该点为止**相同前缀的字符串个数**的**树**，称为 **Trie** 树，**树的高度**为**字符串长度**，如下图(蓝色为边权，红色为点权)。其通常处理**前缀问题**(如判断某个字符串作为前缀出现多少次)，但实际很少用到 **Trie 树**，常用的是 **01Trie 树**  
  > 2、**01Trie** 存储的**不是字符**，而是**二进制**的 $0, 1$，用于**表示数字**，基于**二叉树**：例如**数组内**有数字 $5 = 0101_2, 7 = 0111_2, 2 = 0010_2, 10 = 1010_2$，便可以用**01Trie 树**来存储表示**这个数组**，**树的高度**为**最长二进制位数**，如下图所示  
  > 3、**01Trie**初始只有一个**空节点**(初始节点 $0$)，采用**动态开点**的方式，其**每个节点**存储**点权** $val$ 和**两条边** $0, 1$ 分别指向的**子节点** $son[0], son[1]$。**更新**时，没有的节点就**动态开点**，**经过的节点点权** $+1$，**对应二进制位**为 $0$ 就**左走**，为 $1$ 就**右走**。对于**数值范围**在 $10^9$ 内的(约 $30$ 位二进制)，**01Trie** 通常需要 $32n = N \lt\lt 5$ 的**空间**

  <!-- more -->

  ![](https://cdn.jsdelivr.net/gh/ShanMuYunQiu/Image/blog/%E7%AE%97%E6%B3%95/01Trie.png)

- **最大异或对**

  > 1、[最大异或对](https://www.starrycoding.com/problem/89)：给定一个**长度为** $n$ ($n \leq 2 \times 10^5$) 的**数组** $a[]$，找出一个**二元组** $(a_i, a_j)$，使得 $a_i \oplus a_j$ **最大**并输出  
  > 2、分析可知，从 **01Trie** 起点开始，只要 $a_i, a_j$ 每次**选择相反的路**(一个走 $0$，一个走 $1$)，那么**异或**后**该位结果**为 $1$，否则**该位结果**为 $0$。又因为**01Trie**对于**异或运算**满足**从高位向低位**的**贪心性质**，**向下走**的过程是**从二进制高位向低位**的，所以每次只要**选择相反的路**就是**整体最优**(高位大，数字一定更大)  
  > 3、实现中，注意处理数字要**从高位向低位**处理，一般 **int** 范围从**第** $30$ **位**开始处理，在循环中**更新当前节点** $o$

- **代码实现**

  ```cpp
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 2e5 + 10;
  int a[N];

  struct Node
  {
          int val;    // 点权
          int son[2]; // 左右子节点
  } t[N << 5];        // 01Trie
  int idx;            // 内存池

  // 插入新的值，动态开点
  void insert(int x)
  {
      int o = 0;  // 从节点 0 开始
      t[o].val++; // 点权 val++

      // 从二进制高位向低位
      for (int i = 30; i >= 0; i--)
      {
          int y = x >> i & 1; // 取出 x 的第 i 位

          // 引用，看节点 o 的 son[y] 是否存在，如果不存在，就动态开点
          int &u = t[o].son[y];
          if (!u)
              u = ++idx;

          o = u;      // 向下走
          t[o].val++; // 更新点权，注意一定写在后面，不然叶子是不加的
      }
  }

  int getMax(int x)
  {
      int o = 0;
      int res = 0;

      for (int i = 30; i >= 0; i--)
      {
          int y = x >> i & 1;

          int u = t[o].son[y], v = t[o].son[!y]; // 取出相反的两条边
          // u 是小点，v 是大点，如果 v 存在就尽可能走 v
          if (v)
          {
              o = v;           // 走相反的 v，异或后该位为 1
              res |= (1 << i); // 更新 res
          }
          else
              o = u; // 只能走 u，异或后该位为 0
      }

      return res;
  }

  void solve()
  {
      int n;
      cin >> n;
      for (int i = 1; i <= n; i++)
          cin >> a[i];

      for (int i = 1; i <= n; i++)
          insert(a[i]);

      int ans = 0;
      for (int i = 1; i <= n; i++)
          ans = max(ans, getMax(a[i]));
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

### **01Trie-第 k 小异或对**

---

- **第 k 小异或对**

  > 1、[第 k 小异或对](https://www.starrycoding.com/problem/91)：给定**数组长度** $n$ 和**询问次数** $q$ ($n,q \leq 2 \times 10^5$) ，给出**数组** $a[]$。对于**每次询问**，输入 $t\ x\ k$，输出 $a_1 \oplus x, a_2 \oplus x, ..., a_t \oplus x$ 中**第** $k$ **小的值**  
  > 2、**判断第** $k$ **小**的思路与**权值线段树**的思路相似：从**起点开始**，假设要判断的 $x$ **对应位二进制**为 $y$，**向下走**时，选择**与** $y$ **相同的边**异或后的**结果是更小的**，**与** $y$ **不同的边**异或后的**结果是更大的**。令**相同的边**的**子节点点权**为 $val$，要找**第** $k$ **小**：如果 $val \geq k$，说明**前** $k$ **小**的结果**都在这条边**，应**向小的这边**查找**第** $k$ **小**；否则**向大的一边**走，排除掉另一边的小的，找**第** $k - val$ **小**。**边走边记录**选择的边  
  > 3、另一个问题是，询问的是**区间** $[1, t]$ 的 **01Trie 树**，对于单个这样的**前缀区间**可以处理到 $a_t$ **入树**就对 $[1, t]$ 的 **01Trie 树**进行查找，但该例中，如果**先询问** $[1, 30]$ **后询问** $[1, 10]$，此时的 **01Trie** 已经处理了 $[1, 30]$，不能回头处理 $[1, 10]$ **的情况** (删除回退会使复杂度过高)。所以，应当**提前记录**所有询问**使询问离线**，按 $t$ **升序**的顺序处理，**记录** $id$ 并**根据** $id$ **输出答案**

- **代码实现**

  ```cpp
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 2e5 + 10;
  int a[N];

  struct Node
  {
          int val;    // 点权
          int son[2]; // 左右子节点
  } t[N << 5];        // 01Trie
  int idx;            // 内存池

  // 存储询问
  struct Q
  {
          int id;      // 询问的编号
          int t, x, k; // 询问的内容
  };
  vector<Q> que; // 离线记录询问
  int ans[N];    // 离线保存答案

  // 插入新的值，动态开点
  void insert(int x)
  {
      int o = 0;  // 从节点 0 开始
      t[o].val++; // 点权 val++

      // 从二进制高位向低位
      for (int i = 30; i >= 0; i--)
      {
          int y = x >> i & 1; // 取出 x 的第 i 位

          // 引用，看节点 o 的 son[y] 是否存在，如果不存在，就动态开点
          int &u = t[o].son[y];
          if (!u)
              u = ++idx;

          o = u;      // 向下走
          t[o].val++; // 更新点权，注意一定写在后面，不然叶子是不加的
      }
  }

  int getVal(int x, int k)
  {
      int o = 0;
      int res = 0;

      for (int i = 30; i >= 0; i--)
      {
          int y = x >> i & 1;

          int u = t[o].son[y], v = t[o].son[!y]; // 取出相反的两条边
          // u 点是小点， v 点是大点，判断小点存不存在、有没有 k 个，有的话就走 v
          if (u && t[u].val >= k)
              o = u; // 走 u，不需要更新 res
          else
          {
              o = v;                   // 走 v
              k -= (u ? t[u].val : 0); // 去除另一条路的小点(如果有的话)
              res |= (1 << i);         // 需要更新 res
          }
      }

      return res;
  }

  void solve()
  {
      int n, q;
      cin >> n >> q;
      for (int i = 1; i <= n; i++)
          cin >> a[i];

      // 记录所有询问，使询问离线
      for (int i = 1; i <= q; i++)
      {
          int t, x, k;
          cin >> t >> x >> k;
          que.push_back({i, t, x, k});
      }
      // 按 t 升序排列询问
      sort(que.begin(), que.end(), [](const Q &u, const Q &v) { return u.t < v.t; });

      int now = 0; // 记录此时 Trie 树更新到了哪个元素
      // 遍历询问
      for (auto &qe : que)
      {
          // 继续更新 Trie 树
          while (now < qe.t)
              insert(a[++now]);

          // 此时 Trie 存的就是当前 [1, t] 的信息
          ans[qe.id] = getVal(qe.x, qe.k); // 记录答案
      }

      for (int i = 1; i <= q; i++)
          cout << ans[i] << '\n';
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
