---
title: 倍增LCA与Tarjan
author: 山暮云秋
description: 学习倍增 LCA 与 Tarjan
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
updated: 2024-08-17 00:00:00
---

---

### **LCA**

---

- **LCA 最近公共祖先**

  > 1、[LCA](https://www.starrycoding.com/problem/92)：输入树的**节点个数** $n$，这棵树**以节点** $1$ **为根**，接下来 $n-1$ 行，分别输入节点 $2 \to n$ 的**父节点**，再输入**询问次数** $q$，每个询问给出**两个整数** $u, v$，输出 $u, v$ 的**最近公共祖先** $lca(u,v)$  
  > 2、**最近公共祖先 LCA**：**深度最大**的**公共祖先**，即几个节点的**父节点链**从下向上**最早相交的点** (可以是这两个点中的一个)。**性质**：$u \to v$ 的**简单路径**可以分为**两条链**，即 $u \to lca(u,v)$ 和 $lca(u,v) \to v$；类似于 $max, min, gcd$ 等运算，其**满足** $lca(x,y,z) = lca(x,lca(y,z))$；**多个点**的 $lca(x_1,x_2,...,x_k)$ 等价于 **DFS 序最小点** 和 **DFS 序最大点**的 $lca$  
  > 3、

  <!-- more -->

---

### **页底评论**

---
