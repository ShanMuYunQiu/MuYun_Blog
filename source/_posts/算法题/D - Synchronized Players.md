---
title: D - Synchronized Players
author: 山暮云秋
sticky: 0
readmore: true
tags:
  - BFS
categories:
  - 算法题
abbrlink: d9ea786c
date: 2024-02-09 00:00:00
---

##### [D - Synchronized Players](https://atcoder.jp/contests/abc339/tasks/abc339_d)

<!-- more -->

## 解题思路

1、题意可理解为：有两个`P`代表**两个人**且每次可以**同时同向移动一格**，矩阵中`#`和矩阵**边界**代表**障碍**。如果移动时**遇到障碍**就**停在原地**，求**最少的移动次数**使两人重叠

2、显然，对于**迷宫中求最少移动次数**的问题大都采用 **BFS** 来完成。但与常见的 **BFS** 题目不同的是该题**有两个人**，因此原先标记**是否走过**的`vis[i][j]`和记录**到达(i, j)的最少次数**的`d[i][j]`应同时能表示**两个人的状态**

3、因此，应使用`vis[i][j][k][l]`表示**第一个人**在`(i, j)`且**第二个人**在`(k, l)`有没有走过，用`dp[i][j][k][l]`表示到达`(i, j, k ,l)`状态的最少移动次数。其余的**具体实现**同 **BFS** 的迷宫寻路模板

4、注意在 **BFS** 实现过程中，如果**两个人都原地不动**，需要特判**跳过这种情况**

## 代码实现

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 70;
const int INF = 0x3f3f3f3f;

string mp[N];
bitset<N> vis[N][N][N]; // 标记是否访问过
int d[N][N][N][N];      // 记录到达 (i, j, k, l) 状态的最少移动次数
int dx[] = {1, -1, 0, 0}, dy[] = {0, 0, 1, -1};
int n;

// 处理边界导致的原地不动
void in_map(int &nx1, int &ny1, int &nx2, int &ny2)
{
    if (nx1 < 1)
        nx1 = 1;
    if (nx1 > n)
        nx1 = n;
    if (ny1 < 1)
        ny1 = 1;
    if (ny1 > n)
        ny1 = n;
    if (nx2 < 1)
        nx2 = 1;
    if (nx2 > n)
        nx2 = n;
    if (ny2 < 1)
        ny2 = 1;
    if (ny2 > n)
        ny2 = n;
}

void BFS(int x1, int y1, int x2, int y2)
{
    // q 为待访问的队列，用两对 pair 表示两个点的坐标
    queue<pair<pair<int, int>, pair<int, int>>> q;
    q.push({{x1, y1}, {x2, y2}});

    // 初始化
    vis[x1][y1][x2][y2] = true;
    memset(d, INF, sizeof(d));
    d[x1][y1][x2][y2] = 0;

    // 处理 BFS 队列
    while (!q.empty())
    {
        // 获取当前坐标
        int x1 = q.front().first.first, y1 = q.front().first.second;
        int x2 = q.front().second.first, y2 = q.front().second.second;
        q.pop();

        for (int i = 0; i < 4; i++)
        {
            // 枚举下一次坐标
            int nx1 = x1 + dx[i], ny1 = y1 + dy[i];
            int nx2 = x2 + dx[i], ny2 = y2 + dy[i];
            in_map(nx1, ny1, nx2, ny2); // 处理边界

            // 遇到障碍原地不动
            if (mp[nx1][ny1] == '#')
                nx1 = x1, ny1 = y1;
            if (mp[nx2][ny2] == '#')
                nx2 = x2, ny2 = y2;

            // 如果当前状态已访问，跳过
            if (vis[nx1][ny1][nx2][ny2])
                continue;
            // 特判：如果两个人都不动，则跳过
            if (nx1 == x1 && ny1 == y1 && nx2 == x2 && ny2 == y2)
                continue;

            // 将下一次坐标放入队列，更新操作次数，标记已访问
            q.push({{nx1, ny1}, {nx2, ny2}});
            d[nx1][ny1][nx2][ny2] = d[x1][y1][x2][y2] + 1;
            vis[nx1][ny1][nx2][ny2] = true;
        }
    }
}

void solve()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
    {
        cin >> mp[i];
        mp[i] = ' ' + mp[i];
    }

    // 找到两个人的坐标
    int x1 = 0, y1 = 0, x2 = 0, y2 = 0;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            if (mp[i][j] == 'P')
            {
                if (x1 == 0)
                {
                    x1 = i;
                    y1 = j;
                }
                else
                {
                    x2 = i;
                    y2 = j;
                    break;
                }
            }

    BFS(x1, y1, x2, y2); // BFS 处理

    // 找到最小答案，两人都在 (i,j) 点上
    int ans = INF;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            ans = min(ans, d[i][j][i][j]);

    // 输出答案
    if (ans == INF)
        cout << "-1";
    else
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

## 页底评论
