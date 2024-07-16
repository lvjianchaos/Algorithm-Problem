**比赛场次** [Codeforces Round 915 (Div. 2)](https://codeforces.com/contest/1905)

**比赛题目** [Begginer's Zelda](https://codeforces.com/contest/1905/problem/B)

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

You are given a tree†. In one *zelda-operation* you can do follows:

- Choose two vertices of the tree `u` and `v`;
- Compress all the vertices on the path from `u` to `v` into one vertex. In other words, all the vertices on path from `u` to `v` will be erased from the tree, a new vertex `w `will be created. Then every vertex `s` that had an edge to some vertex on the path from u to `v `will have an edge to the vertex `w`.

<img src="https://espresso.codeforces.com/17652d819e3f2befc596f790b49c7268c923236b.png" alt="img" style="zoom:60%;" />(Illustration of a zelda-operation performed for vertices 11 and 55.)

Determine the minimum number of zelda-operations required for the tree to have only one vertex.

†A tree is a connected acyclic undirected graph.

## 输入

Each test consists of multiple test cases. The first line contains a single integer t (1≤t≤1e4) — the number of test cases. The description of the test cases follows.

The first line of each test case contains a single integer n (2≤n≤1e5) — the number of vertices.

i-th of the next n−1 lines contains two integers u[i] and v[i] (1≤`u[i],v[i]`≤n,u[i]≠v[i]) — the numbers of vertices connected by the i-th edge.

It is guaranteed that the given edges form a tree.

It is guaranteed that the sum of n over all test cases does not exceed 1e5.

## 输出

For each test case, output a single integer — the minimum number of zelda-operations required for the tree to have only one vertex.

## 样例

**输入**

> 4
> 4
> 1 2
> 1 3
> 3 4
> 9
> 3 1
> 3 5
> 3 2
> 5 6
> 6 7
> 7 8
> 7 9
> 6 4
> 7
> 1 2
> 1 3
> 2 4
> 4 5
> 3 6
> 2 7
> 6
> 1 2
> 1 3
> 1 4
> 4 5
> 2 6

**输出**

> 1
> 3
> 2
> 2

## 解释

In the first test case, it's enough to perform one zelda-operation for vertices 22 and 44.

In the second test case, we can perform the following zelda-operations:

1. u=2,v=1. Let the resulting added vertex be labeled as w=10;
2. u=4,v=9. Let the resulting added vertex be labeled as w=11;
3. u=8,v=10. After this operation, the tree consists of a single vertex.

## 题解

##### **贪心  树的基本性质**

要使其变成1个节点，我们会在每一次融合中**尽可能融合更多**的节点，即：从一个叶节点到另一个叶节点的路径上的节点全部融合。也就是——<u>求叶子节点整除2向上取整即可</u>。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll N = 2e5;
ll t[N];
void solve()
{
    // init and input
	memset(t,0,sizeof(t));
	int n;cin >> n;
    // build the tree
	for(int i = 1;i < n;i ++)
	{
		int u,v;cin >> u >> v;
		t[v] ++;
		t[u] ++;
	}
    // to solve
	int leaves_num = 0;
	for(int i = 1;i <= n;i ++)
		if(t[i] == 1) leaves_num ++; // is it the leaf of the tree?
    // output
	cout << ((leaves_num + 1) >> 1) << '\n'; // notice: ceil(leaves_num / 2)
}
int main(void)
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int _;cin >> _;
	while(_ --) solve();
	return 0;
}
```

