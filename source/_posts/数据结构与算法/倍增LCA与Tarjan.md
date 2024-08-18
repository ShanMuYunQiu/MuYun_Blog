---
title: 倍增LCA与Tarjan
author: 山暮云秋
description: 学习倍增 LCA 与 Tarjan，用 Tarjan 求 LCA、强联通分量、割点割边、找环
sticky: 0
readmore: true
tags:
  - LCA
  - Tarjan
  - C++
categories:
  - 数据结构与算法
abbrlink: 8b3a7218
date: 2024-08-17 00:00:00
updated: 2024-08-18 00:00:00
---

---

### **倍增 LCA**

---

- **倍增 LCA**

  > 1、[LCA](https://www.starrycoding.com/problem/92)：输入树的**节点个数** $n$，这棵树**以节点** $1$ **为根**，接下来 $n-1$ 行，分别输入节点 $2 \to n$ 的**父节点**，再输入**询问次数** $q$，每个询问给出**两个整数** $u, v$，输出 $u, v$ 的**最近公共祖先** $lca(u,v)$  
  > 2、**最近公共祖先 LCA**：**深度最大**的**公共祖先**，即几个节点的**父节点链**从下向上**最早相交的点** (可以是这两个点中的一个)。**性质**：$u \to v$ 的**简单路径**可以分为**两条链**，即 $u \to lca(u,v)$ 和 $lca(u,v) \to v$；类似于 $max, min, gcd$ 等运算，其**满足** $lca(x,y,z) = lca(x,lca(y,z))$；**多个点**的 $lca(x_1,x_2,...,x_k)$ 等价于 **DFS 序最小点** 和 **DFS 序最大点**的 $lca$  
  > 3、**倍增 LCA**：类似于**ST 表**，我们令 $fa[x][j]$ 表示**节点** $x$ **向上走** $2^j$ **所到达的点**，例如对于**节点** $x$，其向上**第一个父节点**为 $fa[x][0]$，**第二个**为 $fa[x][1]$，**第四个**为 $fa[x][2]$，**第八个**为 $fa[x][3]$；此外，从根开始以**节点深度**从上到下**标记层次**，用 $dep[x]$ 记录 $x$ **的深度**。这样的结构可以实现**快速跳跃**，要从 $x$ 向上**跳跃到目标层次**，可以**贪心**地**先迈大步再迈小步**：**从大到小**枚举 $j$，再检查 $fa[x][j]$ 是否**超过目标层次**，超过则**不操作**，不超过则 $x$ **跳跃到当前** $fa[x][j]$，继续向下枚举。例如，如果要从**第** $8$ **层**跳跃到**第** $2$ **层**，二者相隔 $6$ 个节点：为方便，从 $j = 3$ **开始枚举**，此时 $fa[x][j]$ 是向上 $8$ **个节点**的位置，超出目标范围不操作；继续**枚举** $j = 2$，此时 $fa[x][j]$ 为向上 $4$ **个节点**的位置，令 $x$ **跳到该位置**，此时**还差** $2$ **个节点**到目标；继续**枚举** $j = 1$，$x$ **跳到当前** $fa[x][j]$ **到达目标节点**。可以发现，起始**相隔的** $6$ **个节点**二进制为 $110_2$，$1$ **的位置**恰好指示了**对应位置** $j$ 需要跳跃  
  > 4、实现上，**初始化**最上面虚拟的点 $fa[1][0] = 0$ (无意义)，操作上需要**求** $fa[][]$ **数组**，再**求** $lca$。对于**求数组**，可以通过 **DFS** 得到，**转移方程**为 $fa[x][j] = fa[fa[x][j-1]][j-1]$，例如求 $fa[x][2] = fa[fa[x][1]][1]$ (求 $x$ 向上走 $2^2$ 步，等同于从 $x$ 向上走 $2^1$ 步再走 $2^1$ 步)，在 **DFS** 过程中，**上面的点**的 $fa[][]$ (即转移方程右侧)是已知的；此外，对于 $dep[]$ 有 $dep[x] = dep[fa[x][0]] + 1$ (深度为父节点 $+1$)。对于**求** $lca$，给定两点 $x, y$，首先将**深度大的**(假设是 $x$)向上跳至二者在**同一深度**(用上面的方法跳跃)，判断 $x, y$ **是否相等**(因为可能 $y$ 就是 $x$ 的祖先)，相等就输出 $y$；如果**不相等**，那么**二者一起向上跳**，依然**从大到小**枚举 $j$，但条件为**若** $fa[x][j] = fa[y][j]$ **则不跳** (始终保持两个点不相等)，这样操作后，最终**两个点**会留在 $lca$ **的子节点**，所以**返回父节点** $fa[x][0]$ 即为 $lca$

  <!-- more -->

- **代码实现**

  ```c++
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 2e5 + 10;
  int n, q;
  vector<int> g[N];  // 建图
  int fa[N][1 << 5]; // fa[x][j] 记录 x 向上 2^j 的节点编号
  int dep[N];        // 记录深度

  // DFS 建造 fa[][] 和 dep[]
  void DFS(int x)
  {
      // 求 fa[][]
      for (int j = 1; j <= 20; j++)
          fa[x][j] = fa[fa[x][j - 1]][j - 1];

      // 深度为父节点 + 1
      dep[x] = dep[fa[x][0]] + 1;

      // 遍历出点，深度优先
      for (auto &y : g[x])
          DFS(y);
  }

  // 求 lca
  int lca(int x, int y)
  {
      // 交换使得 x 深度大
      if (dep[x] < dep[y])
          swap(x, y);

      // 深度大的先跳至相同深度
      for (int j = 20; j >= 0; j--)
          // 如果 x 不会跳过头，即 目标点深度 >= y 的深度
          if (dep[fa[x][j]] >= dep[y])
              // 跳至目标点
              x = fa[x][j];

      // 判断 x,y 是否相等
      if (x == y)
          return y;

      // 不相同，两点一起向上跳，至 lca 的子节点
      for (int j = 20; j >= 0; j--)
          // 只要两点目标节点不相同
          if (fa[x][j] != fa[y][j])
              // 一起向上跳
              x = fa[x][j], y = fa[y][j];

      // 返回父节点即为 lca
      return fa[x][0];
  }

  void solve()
  {
      cin >> n;
      for (int i = 2; i <= n; i++)
      {
          cin >> fa[i][0];          // 输入 fa[i][0] 父节点
          g[fa[i][0]].push_back(i); // 建图
      }

      DFS(1);

      cin >> q;
      while (q--)
      {
          int x, y;
          cin >> x >> y;
          cout << lca(x, y) << '\n';
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

### **Tarjan 求 LCA**

---

- **Tarjan 求 LCA**

  > 1、[LCA](https://www.starrycoding.com/problem/92)：输入树的**节点个数** $n$，这棵树**以节点** $1$ **为根**，接下来 $n-1$ 行，分别输入节点 $2 \to n$ 的**父节点**，再输入**询问次数** $q$，每个询问给出**两个整数** $u, v$，输出 $u, v$ 的**最近公共祖先** $lca(u,v)$  
  > 2、**Tarjan 求** $lca$ 分为两部分：**将询问离线**；**DFS**过程中用**并查集**维护 $lca$。初始并查集都**指向自己**，询问离线后，在 **DFS** 时对于走过的**每个节点**，用 $vis$ **标记**，再**计算深度**；第一步先**处理子节点**，该点**所有子节点**处理后，**递回时**第二步看自己**有没有询问**，第三步将该点的并查集**向上合并**。该算法实际利用了 **DFS** 的**顺序性质**，过程中**并查集的根**即为**下面节点询问**的 $lca$  
  > 3、具体地，如下图例，其中**红色节点**为**离线后的询问**，**红虚线**表示 **DFS 的进度**，**蓝箭头**表示**并查集指向**：图一，**DFS** 到节点 $4$ **遇到询问**，但**没有走到另一个点**($vis$ 未被标记)无法处理；图二，从 $4$ 向上过程中**更新并查集**，到节点 $2$ 发现**子节点还未处理完**，继续**处理子节点**，遇到**另一个询问** $6$，此时**另一个询问** $4$ 已经走过($vis$ 被标记)，那么**这组询问的答案**即为**先前询问节点**(当前节点并查集还没有指向)的**并查集根** $root(4)$；图三，**向上返回**，并查集向上**指向父节点**，直至回到 $2$，再向上时 $2$ **才指向** $1$，图例中省略了后面的处理  
  > 4、通常**倍增** $lca$ **更通用**，**Tarjan** 只能处理**离线询问**，更常用的还是**倍增法**

  ![](https://cdn.jsdelivr.net/gh/ShanMuYunQiu/Image/blog/%E7%AE%97%E6%B3%95/Tarjan%E6%B1%82LCA.png)

- **代码实现**

  ```c++
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 2e5 + 10;

  int n, q;
  vector<int> g[N]; // 建图
  bitset<N> vis;    // 是否走过
  int fa[N];        // 父节点
  int dep[N];       // 节点深度

  vector<pair<int, int>> Q[N]; // 离线询问，Q[x] 存储有关 x 节点的询问，first 为询问 y，second 为询问编号
  int pre[N];                  // 并查集
  int ans[N];                  // 记录答案

  // 路径压缩
  int root(int x)
  {
      return pre[x] = pre[x] == x ? x : root(pre[x]);
  }

  // 合并并查集
  void merge(int x, int y)
  {
      // 将深度大的指向深度小的
      int rx = root(x), ry = root(y);
      if (dep[rx] < dep[ry])
          swap(rx, ry);
      pre[rx] = ry;
  }

  // Tarjan 求 LCA
  void DFS(int x)
  {
      vis[x] = true;           // 标记走过
      dep[x] = dep[fa[x]] + 1; // 处理深度

      // 处理子节点
      for (auto &y : g[x])
          DFS(y);

      // 处理自己的询问
      for (auto &[y, id] : Q[x])
      {
          // 如果 y 还未走到，跳过
          if (!vis[y])
              continue;
          // 否则该询问的结果为当前并查集的根
          ans[id] = root(y);
      }

      // 标记，合并并查集
      merge(x, fa[x]);
  }

  void solve()
  {
      cin >> n;
      for (int i = 2; i <= n; i++)
      {
          cin >> fa[i];
          g[fa[i]].push_back(i); // 建图
      }

      // 初始化并查集
      for (int i = 1; i <= n; i++)
          pre[i] = i;

      cin >> q;
      for (int i = 1; i <= q; i++)
      {
          int x, y;
          cin >> x >> y;
          // 离线询问，记录询问的编号 i
          Q[x].push_back({y, i});
          Q[y].push_back({x, i});
      }

      DFS(1);

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

### **Tarjan 强联通分量**

---

- **Tarjan 强联通分量**

  > 1、

---

### **页底评论**

---
