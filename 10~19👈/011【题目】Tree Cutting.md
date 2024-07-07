**比赛场次** [Codeforces Round 936 (Div. 2)](https://codeforces.com/contest/1946)

**比赛题目** [C. Tree Cutting](https://codeforces.com/contest/1946/problem/C)

<!--more-->

3 seconds / 512 megabytes

standard input / standard output

## 题目

You are given a tree with n vertices.

Your task is to find the maximum number x such that it is possible to remove exactly k edges from this tree in such a way that the size of each remaining connected component†† is at least x.

†† Two vertices v and u are in the same connected component if there exists a sequence of numbers t1,t2,…,tk of arbitrary length k, such that t1=v, tk=u, and for each i from 1 to k−1, vertices ti and ti+1 are connected by an edge.

## 输入

Each test consists of several sets of input data. The first line contains a single integer t (1≤t≤1e4) — the number of sets of input data. This is followed by a description of the sets of input data.

The first line of each set of input data contains two integers n and k (1≤k<n≤1e5) — the number of vertices in the tree and the number of edges to be removed.

Each of the next n−1 lines of each set of input data contains two integers v and u (1≤v,u≤n) — the next edge of the tree.

It is guaranteed that the sum of the values of n for all sets of input data does not exceed 1e5.

## 输出

For each set of input data, output a single line containing the maximum number x such that it is possible to remove exactly k edges from the tree in such a way that the size of each remaining connected component is at least x.

## 样例

**输入**

> 6
> 5 1
> 1 2
> 1 3
> 3 4
> 3 5
> 2 1
> 1 2
> 6 1
> 1 2
> 2 3
> 3 4
> 4 5
> 5 6
> 3 1
> 1 2
> 1 3
> 8 2
> 1 2
> 1 3
> 2 4
> 2 5
> 3 6
> 3 7
> 3 8
> 6 2
> 1 2
> 2 3
> 1 4
> 4 5
> 5 6

**输出**

> 2
> 1
> 3
> 1
> 1
> 2

## 解释

The tree in the first set of input data:

![img](https://espresso.codeforces.com/6267925127de4d26c592c2c57670592afafea67f.png)

After removing the edge 1 — 3, the tree will look as follows:

![img](https://espresso.codeforces.com/eff5a8cc21edcc08b4151404be9a4b84afd18bdd.png)

The tree has split into two connected components. The first component consists of two vertices: 1 and 2. The second connected component consists of three vertices: 3,4 and 5. In both connected components, there are at least two vertices. It can be shown that the answer 3 is not achievable, so the answer is 2.

## 题解

##### **题意**：

给你一棵n节点的树，要求切k条边，找出每段都尽可能大且至少是x

##### **思路**：

类似的：有n个木头切成k段，每段的长度都至少是x且尽可能大

这样，删除k个边就变成k+1个子树，每个子树大小>=x

转化为**二分答案**，由**求最大值**想到二分答案

”最小的最大化“ ----> 二分答案 问题变成：保证最小的连通块最大，求最多能分成多少块，贪心

难点：`DFS`

详见代码。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
// 删除k个边就变成k+1个子树
// ”最小的最大化“ ----> 二分答案 问题变成：保证最小的连通块最大，求最多能分成多少块，贪心
typedef long long ll;
typedef pair<ll, ll> pll;
const ll mod = 1e9 + 7;
const int maxn = 2e6 + 5;

int n, m;
vector<int> e[maxn];
int cnt;

int dfs(int now, int lst, int t) // 分别是现在节点，上一节点，t表示子树最大大小
{
    int num = 1;
    for (auto x: e[now]) 
    {
        if (x == lst) continue; 
        num += dfs(x, now, t);
    }
    if (num >= t) // 如果数量大于子树最大大小，剪掉与父亲的边，返回0
    {
        cnt++;
        return 0;
    }
    return num;
}

bool check(int x) // 如果子树的数量大于m，返回真
{
    cnt = 0;
    dfs(1, -1, x);
    return cnt > m;
}

void solve() 
{
    cin >> n >> m;
    // 清空操作
    for (int i = 1; i <= n; i++) 
    {
        e[i].clear();
    }
    // 建树
    for (int i = 1; i < n; i++) 
    {
        int u, v;
        cin >> u >> v;
        e[u].push_back(v);
        e[v].push_back(u);
    }
    // 二分答案
    int left = 1, right = n;
    while (left + 1 != right) 
    {
        int mid = (left + right >> 1);
        if (check(mid)) left = mid;
        else right = mid;
    }

    if (check(right)) cout << right << endl;
    else cout << left << endl;
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int T = 1;cin >> T;
    while (T--) solve();
}
```

