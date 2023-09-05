---
title: AVL树
author: 圣奇宝枣
description: 学习构建和使用二叉查找树，再进一步学习AVL树
sticky: 0
date: 2023-09-03
updated: 2023-09-05
readmore: true
tags:
  - 二叉查找树
  - AVL树
  - 二叉树
  - C++
categories:
  - 数据结构与算法
---

---

#### **二叉查找树**

---

- **二叉查找树**

  > 1、**二叉查找树**：又称**二叉排序树**/**二叉搜索树**，其**任意节点**与其**左右子节点**的**大小关系**都有`左 < 中 < 右`，其**中序遍历**会得到一个**递增序列**，常用于**元素的排序和查找**，如下图  
  > 2、**查找元素**：**二叉查找树**是以**二分查找**为原型的数据结构，因此其**查找操作**的**时间复杂度**为 O(log<sub>2</sub>n)。其**查找元素**的动作为从**根节点**起，如果**要查找的值**比根小，则继续查找**左子树**，否则查找**右子树**  
  > 3、**插入元素**：与**查找元素**类似。如在下面的**二叉查找树**中**插入新元素 12**，应放在**元素 11**的**右子节点处**(从根节点依次比较 12 与 10、13、11 的关系)  
  > 4、**删除元素**：实质是保持**中序遍历**(即递增序列)的**顺序不变**从中**删除节点**，重点在于如何**填补空缺**。如下图展示了三棵二叉树，分别代表**三种删除情景**：**情景一**要删除**节点 6**(只有一边子树)，可以直接将**子树整体上移**；**情景二**要删除**节点 7**(两边都有子树)，可以将其**中序遍历**的**前/后一个节点**(即节点 6 或 8)填充到此处；**情景三**要删除**节点 10**，如果选择填充**节点 14**(其仍有子树)，则还应**递归处理 14 的空缺**(按照前两种情景选择方法处理)

  <!-- more -->

  ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/二叉查找树.png)
  ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/二叉查找树删除.png)

- **代码实现：查找**

  ```cpp
  #include <iostream>
  #include <memory>
  using namespace std;

  // 树的最大节点个数
  #define MAX_TREE_SIZE 256
  // 树的节点的数据类型
  typedef int ElemType;

  // 树的节点
  typedef struct BinarySearchTree
  {
          ElemType value;                           // 存储的数据
          shared_ptr<BinarySearchTree> left, right; // 指向左子节点、右子节点

          // 构造函数
          BinarySearchTree(ElemType val) : value(val), left(nullptr), right(nullptr)
          {
          }
  } Node;

  // 查找数据
  shared_ptr<Node> Search(shared_ptr<Node> root, ElemType key)
  {
      shared_ptr<Node> cursor = root;

      // 如果未遍历到最后
      while (cursor != nullptr)
      {
          // 如果相同，返回当前元素
          if (key == cursor->value)
              return cursor;
          // 如果所查找元素更小，则向左子树查找
          else if (key < cursor->value)
              cursor = cursor->left;
          // 如果所查找元素更大，则向右子树查找
          else
              cursor = cursor->right;
      }
      // 未找到
      return nullptr;
  }
  ```

- **代码实现：插入**

  ```cpp
  // 插入数据：返回 bool 值表示是否插入成功
  bool Insert(shared_ptr<Node> root, ElemType key)
  {
      // 一个元素为 key 的节点
      shared_ptr<Node> newNode = make_shared<Node>(key);

      // 如果为空树，当前节点作为该空树的根节点
      if (root == nullptr)
      {
          root = newNode;
          return true;
      }

      // 查找插入位置，cursor 用于辅助查找，parent 记录将插入在谁之下
      shared_ptr<Node> cursor = root, parent = nullptr;
      while (cursor != nullptr)
      {
          parent = cursor;

          // 如果相同，返回 false，当前元素已存在
          if (key == cursor->value)
              return false;
          // 如果所查找元素更小，则向左子树查找
          else if (key < cursor->value)
              cursor = cursor->left;
          // 如果所查找元素更大，则向右子树查找
          else
              cursor = cursor->right;
      }

      // 执行插入操作
      if (key < parent->value)
          parent->left = newNode;
      else
          parent->right = newNode;

      // 返回 true，表示插入成功
      return true;
  }
  ```

- **代码实现：删除**

  ```cpp
  // 删除数据：返回 bool 值表示是否删除成功
  bool Remove(shared_ptr<Node> root, ElemType key)
  {
      if (root == nullptr)
          return false;

      // 查找删除位置，cursor 用于辅助查找，romoveNode 为待删除节点，parent 为其父节点
      shared_ptr<Node> removeNode = nullptr, parent = nullptr, cursor = root;
      while (cursor != nullptr)
      {
          if (key == cursor->value)
          {
              removeNode = cursor;
              break;
          }

          parent = cursor;
          if (key < cursor->value)
              cursor = cursor->left;
          else
              cursor = cursor->right;
      }

      // 找不到待删除节点，返回 false，
      if (removeNode == nullptr)
          return false;

      // 待删除节点有一边子树为空 或 两边都为空(也适用)
      if (removeNode->left == nullptr || removeNode->right == nullptr)
      {
          shared_ptr<Node> fixNode = nullptr; // 待删除节点的替代节点

          // 待删除节点左子树为空，用其右子树替代；右子树为空，用其左子树替代
          if (removeNode->left == nullptr)
              fixNode = removeNode->right;
          else
              fixNode = removeNode->left;

          // 删除并替换节点
          if (parent != nullptr)
          {
              if (parent->left == removeNode)
                  parent->left = fixNode;
              else
                  parent->right = fixNode;
          }
          // 否则 parent 为空，说明删除的是根节点，fixNode 就是新的根节点
          else
              root = fixNode;
      }
      // 待删除节点左右子树都存在，则按照中序遍历查找该节点的替代节点(中序遍历顺序中的前一个或后一个节点)
      // 中序遍历顺序中的前一个节点，即待删除节点左子树的最下层的右子节点；后一个节点，即待删除节点右子树的最下层的左子节点
      else
      {
          // fixNode 为待删除节点的替代节点，fixNodeParent 为替代节点的父节点
          shared_ptr<Node> fixNode = removeNode, fixNodeParent = parent;

          // 查找前一个节点，即左子树的最下层的右子节点(也可以自行实现，查找使用后一个节点替换)
          cursor = removeNode->left;
          while (cursor != nullptr)
          {
              fixNodeParent = fixNode;
              fixNode = cursor;
              cursor = cursor->right;
          }

          // 如果替代节点仍然有左/右子树，那么递归处理替代节点，即在 fixNodeParent 这棵子树中删除 fixNode(执行后这两个节点的链接断开)
          Remove(fixNodeParent, fixNode->value);

          // 替换待删除节点
          if (parent != nullptr)
          {
              if (parent->left == removeNode)
                  parent->left = fixNode;
              else
                  parent->right = fixNode;
          }
          // 否则 parent 为空，说明删除的是根节点，fixNode 就是新的根节点
          else
              root = fixNode;

          // 替换节点的左右子树，指向被删除节点的左右子树
          fixNode->left = removeNode->left;
          fixNode->right = removeNode->right;
          // 断开被删除节点的左右子树链接
          removeNode->left = nullptr;
          removeNode->right = nullptr;
      }

      // 返回 true，表示删除成功
      return true;
  }
  ```

---

#### **AVL 树**

---

---

#### **页底评论**

---
