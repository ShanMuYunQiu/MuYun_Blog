---
title: AVL树与哈夫曼树
author: 圣奇宝枣
description: 学习构建和使用二叉查找树，再进一步学习 AVL 树。了解哈夫曼树和哈夫曼编码
sticky: 0
date: 2023-09-03
updated: 2023-09-14
readmore: true
tags:
  - 二叉查找树
  - AVL树
  - 哈夫曼树
  - 树
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

  ```cpp
  #include <iostream>
  #include <memory>
  using namespace std;

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
  ```

- **代码实现：查找**

  ```cpp
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

- **AVL 树**

  > 1、**AVL 树**：又称**平衡二叉树**，是**二叉查找树**的一种**改进形式**。其**任意节点**的**左右子树高度差不超过 1**，且仍拥有**二叉查找树**的**节点大小关系**，如下图  
  > 2、**插入或删除元素**时，会**动态调整**树的结构，以便提高下次的**搜索效率**(解决了二叉查找树搜索次数不均衡，最极端情况下甚至会变为链表的问题)  
  > 3、其代码实现如下，**平衡因子**(值为 -1 / 0 / 1)为**左子树高度**减去**右子树高度**。其**插入和删除操作**需要调整**最小不平衡树**(即平衡因子的绝对值大于 1 的子树)，平衡的方法便是进行**对应方案的旋转**

  ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/AVL树.png)

  ```cpp
  #include <iostream>
  #include <memory>
  using namespace std;

  // 树的节点的数据类型
  typedef int ElemType;

  // 树的节点
  typedef struct AVLNode
  {
          ElemType value;                  // 存储的数据
          shared_ptr<AVLNode> left, right; // 指向左子节点、右子节点
          int balance;                     // 平衡因子
  } Node;
  ```

- **平衡旋转方案**

  > 1、**右单旋转**：**LL 平衡旋转**，第一个 L 表示**当前节点**(11)的**左子节点**(8)，第二个 L 表示**左子节点**(8)的**左子树**(7)不平衡。这种情况下进行**右单旋转**(向右旋转一次)，如下图  
  > 2、**左单旋转**：**RR 平衡旋转**，第一个 R 表示**当前节点**(14)的**右子节点**(15)，第二个 R 表示**右子节点**(15)的**右子树**(16)不平衡。这种情况下进行**左单旋转**(向左旋转一次)，如下图  
  > 3、**先左后右双旋转**：**LR 平衡旋转**，第一个 L 表示**当前节点**(15)的**左子节点**(8)，第二个 R 表示**左子节点**(8)的**右子树**(12-左 10/右 13)不平衡。这种情况下进行**先左后右双旋转**(先向左旋转一次，再向右旋转一次)，如下图  
  > 4、**先右后左双旋转**：**RL 平衡旋转**，第一个 R 表示**当前节点**(15)的**右子节点**(19)，第二个 L 表示**右子节点**(19)的**左子树**(17-左 16/右 18)不平衡。这种情况下进行**先右后左双旋转**(先向右旋转一次，再向左旋转一次)，如下图  
  > 5、**所旋转的节点**都是**第一个字母指代的节点**，即**当前节点**的**左/右子节点**，将其提升为**父节点**；**其余节点**如果能**跟随旋转**(且不影响大小关系)则**直接跟随**，否则**重新安排合适位置**

  ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/AVL树单旋转.png)
  ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/AVL树LR旋转.png)
  ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/AVL树RL旋转.png)

---

#### **哈夫曼树**

---

- **哈夫曼树**

  > 1、**哈夫曼树**：又叫**最优二叉树**，指**带权路径长度最短**的树。含有**n 个带权叶子节点**的**二叉树**中，**带权路径长度最短**(WPL 最短)的**二叉树**，称为**最优二叉树**或**哈夫曼树**  
  > 2、**权**指对实体(节点/边)属性的**数值化描述**；**节点带权路径长度**指**节点到根的路径长度**与**节点的权**的乘积；**树的带权路径长度**(WPL)指树中**所有叶子结点**的**带权路径长度之和**  
  > 3、**同一棵树**可能有**多种哈夫曼树**，其**不是唯一的**。如下图，列出了**同一棵树不同排列**下的**带权路径长度**计算，其中第 2、3 棵都为**哈夫曼树**

  ```cpp
  #include <iostream>
  using namespace std;

  // 动态分配数组存储
  // 树的节点
  typedef struct HTNode
  {
          int weight;                 // 节点的权值
          int parent, left, right;    // 节点的父节点和子节点下标
  } Node;
  ```

  ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/哈夫曼树.png)

- **构造哈夫曼树**

  > 1、将**所有节点**看作**仅含一个节点的树**，则**所有树**构成了**森林**  
  > 2、**创建新节点**，选择**两个权值最小的树**作为其**左右子树**，**新节点权值**为**二者之和**  
  > 3、将**构成的新树放回森林**中，重复上述步骤直到**只剩一棵树为止**  
  > 4、规律：**权值越大的节点越靠上**，这样**节点到根的路径更短**，因此**该节点带权路径长度也更短**

  ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/哈夫曼树构造.png)

- **哈夫曼编码**

  - **引入**

    > 1、**哈夫曼编码**是一种对**字符编码**进行**压缩和解析**的方式标准，如下图给出了一个例子  
    > 2、其中，**固定长度编码**保留了**完整的 ASCII 码信息**，但传输时过于繁琐，因此我们可以在编码中**对前缀 0 进行压缩**，即**可变长度编码**  
    > 3、但**可变长度编码**解析时，难以确定**前缀 0 的个数**和**分组区别**，**解析十分困难**。其中的原因之一是：**压缩后的字符编码**可能是**另一编码中的一部分**(如 48 压缩后的 110000 是 97 压缩后的 1100001 的一部分)，这便**难以分组**  
    > 4、这时，需要一种**既能压缩也能解析**的**编码方式**，这便是**哈夫曼编码**(前缀编码)。其**任意两个字符的编码**，**不能是对方的前缀**。

    ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/哈夫曼编码1.png)

  - **哈夫曼编码与哈夫曼树**

    > 1、树的**每个节点都是唯一的**，**根节点到叶子结点的路径也是唯一的**。令**哈夫曼树**的**叶子节点**代表不同的**字符**，**左右分支**分别标记**0 和 1**，那么，**根节点到叶节点的路径**就构成了一个**二进制编码**，如下图  
    > 2、**根节点到叶节点的路径**代表了**编码的长度**，**节点的权值**即**字符**(在电文/数据/字符集中)**出现的次数**，这样，**电文的总长度**即为 WPL  
    > 3、由于**哈夫曼树**中**权值越大的节点越靠上**，所以**出现频率高**的字符，其**编码也更短**

    ![](https://cdn.jsdelivr.net/gh/ShengQiBaoZao/Image/blog/算法/哈夫曼编码2.png)

---

#### **页底评论**

---
