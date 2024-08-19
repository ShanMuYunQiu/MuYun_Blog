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
updated: 2024-08-19 00:00:00
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

### **Tarjan LCA**

---

- **Tarjan LCA**

  > 1、[LCA](https://www.starrycoding.com/problem/92)：输入树的**节点个数** $n$，这棵树**以节点** $1$ **为根**，接下来 $n-1$ 行，分别输入节点 $2 \to n$ 的**父节点**，再输入**询问次数** $q$，每个询问给出**两个整数** $u, v$，输出 $u, v$ 的**最近公共祖先** $lca(u,v)$  
  > 2、**Tarjan LCA** 分为两部分：**将询问离线**；**DFS**过程中用**并查集**维护 $lca$。初始并查集都**指向自己**，询问离线后，在 **DFS** 时对于走过的**每个节点**，用 $vis$ **标记**，再**计算深度**；第一步先**处理子节点**，该点**所有子节点**处理后，**递回时**第二步看自己**有没有询问**，第三步将该点的并查集**向上合并**。该算法实际利用了 **DFS** 的**顺序性质**，过程中**并查集的根**即为**下面节点询问**的 $lca$  
  > 3、具体地，如下图例，其中**红色节点**为**离线后的询问**，**红虚线**表示 **DFS 的进度**，**蓝箭头**表示**并查集指向**：图一，**DFS** 到节点 $4$ **遇到询问**，但**没有走到另一个点**($vis$ 未被标记)无法处理；图二，从 $4$ 向上过程中**更新并查集**，到节点 $2$ 发现**子节点还未处理完**，继续**处理子节点**，遇到**另一个询问** $6$，此时**另一个询问** $4$ 已经走过($vis$ 被标记)，那么**这组询问的答案**即为**先前询问节点**(当前节点并查集还没有指向)的**并查集根** $root(4)$；图三，**向上返回**，并查集向上**指向父节点**，直至回到 $2$，再向上时 $2$ **才指向** $1$，图例中省略了后面的处理  
  > 4、通常**倍增** $lca$ **更通用**，**Tarjan LCA** 只能处理**离线询问**，更常用的还是**倍增法**

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

  // Tarjan LCA，本质是 DFS
  void Tarjan_LCA(int x)
  {
      vis[x] = true;           // 标记走过
      dep[x] = dep[fa[x]] + 1; // 处理深度

      // 处理子节点
      for (auto &y : g[x])
          Tarjan_LCA(y);

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

      Tarjan_LCA(1);

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

- **Tarjan 简述**

  > 1、**Tarjan** 指的是**一系列算法**，包括求**强联通分量**、**割点割边**、**找环**等。本文不讲解 **Tarjan** 的原理，只介绍**具体实现**的方式，下面先解释部分名词  
  > 2、**强联通分量 SCC**：图中的一个个**任意两点**都可以**互相到达**的**最大的连通块**(只要是满足条件的最大的连通块都是，并非只能有一个强联通分量)，**单个点**也是一个**强联通分量**。**无向图**和**有向图**中都可以存在：**无向图**的**强联通分量**就是**子连通块**，因为无向图**连通的点**都可以**互相到达**，通常不讨论无向图；**有向图**中需要注意两点**能否互相到达**，**单独的点**也是一个**强联通分量**，如下图中有 $6$ 个**强联通分量**(红色、黄色、$4$ 个单独的黑色)  
  > 3、**割点 cut**、**割边 bridge**：**仅存在于无向图**，**有向图**没有此概念。一个点若是**割点**，那么**删除该点**和**与之相连的边**后，图**不再连通**(图中连通块数量 $+1$)；一条边若是**割边**(桥)，那么**删除该边**后，图**不再连通**。如下图，红色的都是**割点**，绿色的都是**割边**

  ![](https://cdn.jsdelivr.net/gh/ShanMuYunQiu/Image/blog/%E7%AE%97%E6%B3%95/Tarjan.png)

- **Tarjan 强联通分量**

  > 1、[有向图缩边](https://www.starrycoding.com/problem/93)：输入**点数** $n$ 和**边数** $m$ ($n,m \leq 2 \times 10^5$)，对于**每条边**，给出 $x, y$ 表示存在一条 $x$ **到** $y$ **的单向边**。输出所有**大小** $\gt 1$ 的**强联通分量**的**大小**，按**从小到大的顺序**输出。注意，图不保证连通  
  > 2、此处以**有向图**为例，**无向图**将其**看作有向图**实现即可。**Tarjan** 本质是一个 **DFS**，所以会构成一棵**搜索树**，我们通过**给节点染色**区分**强联通分量**，**颜色相同的点**则在**同一个强联通分量**。对于**搜索**，用 $dfn[]$ 存储**节点的时间戳**(被访问的次序)，一般认为**强联通分量**中 $dfn$ **最小的点**是**根节点**(搜索树的根)，$dfn[]$ 只初始更新一次，后续是不变的，实际就是 **DFS 序**，如果 $dfn[x] = 0$ 表示**节点** $x$ **从未走过**；用 $low[]$ 存储**节点能到达的回点**(一个走过的点)的**最小时间戳**(即指向最小的回点)，**初始时** $low[x] = dfn[x]$ 指向自己，后续始终 $low[x] \leq dfn[x]$ (不可能比自己更大)；用 $idx$ 表示**此时的时间戳**。对于**染色**，需要**栈** $stk[]$ 存储**走过且未被染色**的点，$col[]$ 存储**节点的颜色**(可以不用)，$scc$ 表示当前的**颜色总数**(强联通分量总数)  
  > 3、**算法的流程**如下：首先，走到一个**新节点** $x$，记录时间戳，**更新** $dfn[x]$ 和 $low[x]$，初始时 $low[x] = dfn[x] = ++idx$，**将自身入栈**。之后，**向下看子节点** $y$ **是否是回点**：如果**是回点**($dfn[y] \neq 0$ 且在栈中)，不向下走，用**回点**的 $dfn[y]$ **更新** $low[x] = min(low[x], dfn[y])$；如果**不是回点**，继续向下走，在后续**递回时更新** $low[x] = min(low[x], low[y])$ (此时已经知道该点子节点的 $low[y]$ 了)。当发现某个节点的**子节点处理完**且 $low[x] = dfn[x]$ 时，即**该点是一个根**，那么就进行**收拢**，把下面以它为根的**连通块**作为一个**强联通分量**，将**栈中元素取出染色**，直至自己也完成染色  
  > 4、如下图例，为方便理解，**虚构一棵搜索树**(按 DFS 序建树，不考虑闭回的边，由于 DFS 序可能不同所以树的形式不固定)，在**图上的 DFS** 实际就是**对这棵树 DFS**，如右图的过程中蓝色箭头所示。图中节点左侧为 $dfn$，右侧为 $low$，蓝色箭头表示 **DFS 走过去**，红虚箭头表示**获取了下一节点**但**没有走过去**，黄虚箭头表示 **DFS 递回**。图一，按 **DFS** 到**节点** $6$，过程中**初始** $dfn, low$ 为**时间戳** $idx$，将经过的**未染色节点入栈**；红虚箭头处，在**节点** $6$ 下一个点 $2$ 的 $dfn[2] \neq 0$ (先前走过)，更新 $low[6] = min(low[6], dfn[2]) = 2$，表示该点向上**最远可以回到** $dfn = 2$ 的点位置。图二，**DFS 递回**到节点 $3$，更新 $low[3] = min(low[3], low[6]) = 2$，继续递回到节点 $2$，但此时**子节点未处理完**，继续处理。图三图四的处理同上，最后**递回节点** $2$，发现**子节点已处理完**且 $dfn[2] = low[2] = 2$，该点为**根**开始**收拢**，**被收拢的点**恰是**搜索树**中 $2$ **的子树**所有点，将它们**染色**记为同一强联通分量，即对**栈中节点染色**，直至 $2$ **被染色**。后续处理同上，在此省略  
  > 5、上例是**较为简单的情况**(只有一棵子树)，如果出现**另一棵子树**指向**先前处理过的强联通分量**，例如下图原图中 $7$ **指向** $4$，处理 $7$ 时，**不能向** $4$ **DFS 搜索**：从图上看，因为**无法从** $4$ **回到** $7$；从实现看，这二者**在搜索树上不连通**，先前处理完**左侧强联通分量**后**节点出栈**，就看作已经完结，**搜索树**上**这棵子树看作被删除**，那么**搜索树上** $7 \to 4$ **的边不存在**。具体地，如先前上面第三条所言，节点**向下看之前**，要检查**节点**(在搜索树意义上)**是否存在**，即检查**是否在栈内**，实现中用 $ins$ 标记节点**是否在栈内**

  ![](https://cdn.jsdelivr.net/gh/ShanMuYunQiu/Image/blog/%E7%AE%97%E6%B3%95/Tarjan%E5%BC%BA%E8%81%94%E9%80%9A%E5%88%86%E9%87%8F.png)

- **代码实现**

  ```c++
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 2e5 + 10;

  vector<int> g[N];        // 建图
  int dfn[N], low[N], idx; // Tarjan 搜索的变量
  int col[N], scc;         // Tarjan 染色的变量
  int stk[N], top;         // 维护栈
  bitset<N> ins;           // 是否入栈
  int cnt[N];              // 记录强联通分量大小

  // Tarjan 强联通分量，本质是 DFS
  void Tarjan_SCC(int x)
  {
      // 初始化 dfn 和 low
      dfn[x] = low[x] = ++idx;
      // 入栈并标记
      stk[++top] = x;
      ins[x] = true;

      // 处理子节点
      for (auto &y : g[x])
      {
          // 如果没走过 dfn[y] = 0，一定是儿子不是回点，先走它，走完递回时用 low[y] 更新 low[x]
          if (!dfn[y])
          {
              Tarjan_SCC(y);
              low[x] = min(low[x], low[y]);
          }
          // 否则如果是回点：dfn[y] != 0 且 在栈中，向下看一眼并用 dfn[y] 更新 low[x]
          else if (ins[y])
              low[x] = min(low[x], dfn[y]);
      }

      // 看自己是不是根，收拢
      if (dfn[x] == low[x])
      {
          // 新开一个颜色(计数新的强联通分量)
          scc++;
          // 弹出栈并染色，直到 x 也弹出 (top + 1 为了延迟一个点，让 x 本身也被处理)
          while (stk[top + 1] != x)
          {
              col[stk[top]] = scc;     // 染色
              cnt[scc]++;              // 强联通分量大小 +1
              ins[stk[top--]] = false; // 弹出栈顶并取消标记
          }
      }
  }

  void solve()
  {
      int n, m;
      cin >> n >> m;
      for (int i = 1; i <= m; i++)
      {
          int x, y;
          cin >> x >> y;
          g[x].push_back(y);
      }

      // 图不保证连通
      for (int i = 1; i <= n; i++)
          // 如果该点没走过，就跑一次 Tarjan
          if (!dfn[i])
              Tarjan_SCC(i);

      // 存储所有大小 > 1 的强联通分量大小
      vector<int> ans;
      for (int i = 1; i <= scc; i++)
          if (cnt[i] > 1)
              ans.push_back(cnt[i]);

      // 输出答案
      if (ans.size())
      {
          sort(ans.begin(), ans.end());
          for (auto &a : ans)
              cout << a << ' ';
      }
      else
          cout << -1 << '\n';
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

### **Tarjan 割点割边**

---

- **Tarjan 割点割边**

  > 1、[割点割边](https://www.starrycoding.com/problem/94)：输入**点数** $n$ 和**边数** $m$ ($n,m \leq 2 \times 10^5$)，对于**每条边**，给出 $x, y$ 表示存在一条 $x$ **到** $y$ **的双向边**。输出图中的**割点数量**和**割边数量**  
  > 2、判断**割点**：如果**一个点**在**搜索树**中有**两个及以上**的**不回子节点**(独立子节点)，即假如把**该节点看作根**，其至少有**两串子树**互相**不可到达**(在图中，非根节点向上的父亲链连通的部分也可以看作子树)，该点即为**割点**。例如有 $1 - 2 - 3 - 4 - 5 - 3$ 五条**无向边**：**节点** $2$ 有**子树** $1$ 和**子树** $3$ **互相不可到达**，是一个**割点**；**节点** $3$ 有**子树** $2$ 和**子树** $4$ **互相不可到达**($5$ 只是回到 $3$，没有连通 $3$ 之上的节点)，是一个**割点**；而**节点** $4$ 的**两棵子树** $3$ 和 $5$ **有连通**，所以**不是割点**  
  > 3、实现上，先前的**Tarjan SCC**可以**将图缩边**，判断出**完全相通**的部分，所以只需要在先前代码修改。对于**非根节点** $x$，只需要找到**一个子节点** $y$ **使** $low[y] \geq dfn[x]$ **便是割点**(只要 $x$ 有一串向下的子树 $y$ 不能回到上面父节点子树，即条件中 $y$ 只能回到 $x$ 自己及以下，就有了两串互相不可到达的子树)；对于**根节点**，就需要**两个子节点**满足条件。由于不需要求**强联通分量**，所以不需要**染色**和**栈**。此外注意，在**无向图**上 **DFS** 一定**要判父节点**，不能往回走  
  > 4、判断**割边**：与**割点**几乎相同，不同的是，在从 $x$ **向下走一条边**时，如果**相连的节点** $y$ **的子树**连 $x$ 自己**都回不到**，就是一条**割边**。所以只要满足 $low[y] \gt dfn[x]$ 就是**割边**，不需要判断不回子节点个数

- **代码实现**

  ```cpp
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 2e5 + 10;

  vector<int> g[N];        // 建图
  int dfn[N], low[N], idx; // Tarjan 搜索的变量
  int cut, bri;            // 记录割点和割边数量

  // Tarjan 割点，如果 fa = 0 那么 x 是根
  void Tarjan_Cut(int x, int fa)
  {
      // 初始化 dfn 和 low
      dfn[x] = low[x] = ++idx;

      int child = 0; // 记录不回子节点个数

      // 处理子节点
      for (auto &y : g[x])
      {
          // 不走父亲
          if (y == fa)
              continue;

          // 如果没走过就走，走完递回时用 low[y] 更新 low[x]
          if (!dfn[y])
          {
              Tarjan_Cut(y, x);
              low[x] = min(low[x], low[y]);
              child += (low[y] >= dfn[x]); // 满足不回子节点条件，就记录 +1
          }
          // 只判断是否有回点不求强联通分量，不需要判断栈
          else
              low[x] = min(low[x], dfn[y]);
      }

      // fa == 0 是根需要 2 个，fa != 0 只需要 1 个，是割点
      if ((!fa && child >= 2) || (fa && child >= 1))
          cut++;
  }

  // Tarjan 割边，如果 fa = 0 那么 x 是根
  void Tarjan_Bri(int x, int fa)
  {
      // 初始化 dfn 和 low
      dfn[x] = low[x] = ++idx;

      // 处理子节点
      for (auto &y : g[x])
      {
          // 不走父亲
          if (y == fa)
              continue;

          // 如果没走过就走，走完递回时用 low[y] 更新 low[x]
          if (!dfn[y])
          {
              Tarjan_Bri(y, x);
              low[x] = min(low[x], low[y]);
              bri += (low[y] > dfn[x]); // y 回不到 x 及父亲树上，记录割边
          }
          // 只判断是否有回点不求强联通分量，不需要判断栈
          else
              low[x] = min(low[x], dfn[y]);
      }
  }

  void solve()
  {
      int n, m;
      cin >> n >> m;
      for (int i = 1; i <= m; i++)
      {
          int x, y;
          cin >> x >> y;
          g[x].push_back(y);
          g[y].push_back(x);
      }

      // 求割点，图不保证连通
      for (int i = 1; i <= n; i++)
          // 如果该点没走过，就跑一次 Tarjan
          if (!dfn[i])
              Tarjan_Cut(i, 0);

      // 重置 dnf, low, idx
      for (int i = 1; i <= n; i++)
          dfn[i] = low[i] = 0;
      idx = 0;

      // 求割边，图不保证连通
      for (int i = 1; i <= n; i++)
          // 如果该点没走过，就跑一次 Tarjan
          if (!dfn[i])
              Tarjan_Bri(i, 0);

      cout << cut << ' ' << bri;
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

### **Tarjan 找环**

---

- **Tarjan 找环**

  > 1、[找环](https://www.starrycoding.com/problem/95)：输入**样例组数** $T$ ($T \leq 10^3$)。每组样例，输入**点数和边数** $n$ ($n \leq 2 \times 10^5$)，对于**每条边**，给出 $x, y$ 表示存在一条 $x$ **到** $y$ **的双向边**。图中不含重边与自环，可以证明图中有且仅存在一个环，输出**环的大小**  
  > 2、**无向图找环**，同样用求**强联通分量**的方式，在求**无向图强联通分量**过程中**转换成有向图**，在向下 **DFS** 的过程中，由于**不会向回走父节点**，会**赋予边一个方向**。$n$ **点** $n$ **边**的图，求**强联通分量**一定会**求到环**，其余大多**强联通分量**大小为 $1$，如果遇到不为 $1$ 的即为**环的大小**

- **代码实现**

  ```cpp
  #include <bits/stdc++.h>
  using namespace std;

  const int N = 2e5 + 10;

  vector<int> g[N];

  int dfn[N], low[N], idx;
  int stk[N], top;
  bitset<N> ins;
  int ans;

  // Tarjan 找环
  void Tarjan_Cir(int x, int fa)
  {
      dfn[x] = low[x] = ++idx;
      stk[++top] = x;
      ins[x] = true;

      // 处理子节点
      for (auto &y : g[x])
      {
          if (y == fa)
              continue;

          if (!dfn[y])
          {
              Tarjan_Cir(y, x);
              low[x] = min(low[x], low[y]);
          }
          else if (ins[x])
              low[x] = min(low[x], dfn[y]);
      }

      // 当前点是根
      if (low[x] == dfn[x])
      {
          int cnt = 0; // 记录大小
          
          // 出栈记录大小
          while (stk[top + 1] != x)
          {
              cnt++;
              ins[stk[top--]] = false;
          }

          // 大小 > 1 的强联通分量即为环的大小
          if (cnt > 1)
          {
              ans = cnt;
              return;
          }
      }
  }

  void solve()
  {
      int n;
      cin >> n;

      // 初始化，置空清零
      for (int i = 1; i <= n; i++)
      {
          g[i].clear();
          stk[i] = dfn[i] = low[i] = 0;
          ins[i] = false;
      }
      stk[n + 1] = 0;
      ans = idx = top = 0;

      for (int i = 1; i <= n; i++)
      {
          int x, y;
          cin >> x >> y;
          g[x].push_back(y);
          g[y].push_back(x);
      }

      // 一定有环，图一定连通
      Tarjan_Cir(1, 0);

      cout << ans;
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
