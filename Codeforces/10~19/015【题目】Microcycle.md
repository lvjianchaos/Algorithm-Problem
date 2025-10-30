**比赛场次** [Codeforces Round 923 (Div. 3)](https://codeforces.com/contest/1927)

**比赛题目 ** [F. Microcycle](https://codeforces.com/contest/1927/problem/F)

<!--more-->

4 seconds / 256 megabytes

standard input / standard output

## 题目

Given an undirected weighted graph with n vertices and m edges. There is at most one edge between each pair of vertices in the graph, and the graph does not contain loops (edges from a vertex to itself). The graph is not necessarily connected.

A cycle in the graph is called simple if it doesn't pass through the same vertex twice and doesn't contain the same edge twice.

Find any simple cycle in this graph in which the weight of the lightest edge is minimal.

## 输入

The first line of the input contains a single integer t (1≤t≤1e4) — the number of test cases. Then follow the descriptions of the test cases.

The first line of each test case contains two integers n and m (3≤n≤m≤min(nx(n−1)/2,2e5)) — the size of the graph and the number of edges.

The next m lines of the test case contain three integers u, v, and w (1≤u,v≤n, u≠v, 1≤w≤1e6) — vertices u and v are connected by an edge of weight w.

It is guaranteed that there is at most one edge between each pair of vertices. Note that under the given constraints, there is always at least one simple cycle in the graph.

It is guaranteed that the sum of the values of m for all test cases does not exceed 2⋅1e5.

## 输出

For each test case, output a pair of numbers b and k, where:

- b — the minimum weight of the edge in the found cycle,
- k — the number of vertices in the found cycle.

On the next line, output k numbers from 1 to n — the vertices of the cycle in traversal order.

Note that the answer always exists, as under the given constraints, there is always at least one simple cycle in the graph.

## 样例

**输入**

> 5
> 6 6
> 1 2 1
> 2 3 1
> 3 1 1
> 4 5 1
> 5 6 1
> 6 4 1
> 6 6
> 1 2 10
> 2 3 8
> 3 1 5
> 4 5 100
> 5 6 40
> 6 4 3
> 6 15
> 1 2 4
> 5 2 8
> 6 1 7
> 6 3 10
> 6 5 1
> 3 2 8
> 4 3 4
> 5 3 6
> 2 6 6
> 5 4 5
> 4 1 3
> 6 4 5
> 4 2 1
> 3 1 7
> 1 5 5
> 4 6
> 2 3 2
> 1 3 10
> 1 4 1
> 3 4 7
> 2 4 5
> 1 2 2
> 4 5
> 2 1 10
> 3 1 3
> 4 2 6
> 1 4 7
> 2 3 3

**输出**

> 1 3
> 1 2 3 
> 3 3
> 6 4 5 
> 1 5
> 4 2 1 6 3 
> 1 4
> 1 4 3 2 
> 3 3
> 2 3 1 

## 题解

##### **题意**：

给定一个**无向加权图**，图中有 n个顶点和 m条边。图中每对顶点之间最多有一条边，图中不包含循环(从顶点到自身的边)。该图不一定连通。（如果图中的循环不经过同一顶点两次，也不包含相同的边两次，则称为简单循环）找出该图中最轻边的权重最小的**简单循环**。

##### **思路**：

##### 【**并查集判环 + `dfs`找最小的边权**】

2种做法：`targan` / 并查集 --> 判环

并查集做法

枚举边 i ，对应2个点u , v。find(u) == find(v) 即已经在集合中了，加入边i那么会形成一个环。

题目要求我们最小的边权尽可能小

1）边权从大到小排序

2）枚举边i ， 保证了边权w[i]是当前的并查集中所有边权里面**最小**的

加入第i条边之后，有环，当前边权可能作为答案，枚举直至结束

mn表示最小的在环里的边权

id表示相应mn的编号

mn->id 	id=>u , v

建图，去掉第id条边，从起点u-->终点v的路径

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
// 并查集判环 + `dfs`找最小的边权
ll n,m,root[200005],vis[200005]; // 分别是节点数 边数 根 判断是否搜索过
struct edge{ll u,v,w;}a[200005]; // 存边的信息
vector<ll> e[200005],ans; // 建图 和 路径

ll find(ll u) // 并查集查找根节点(路径压缩)
{
    if(root[u] != u) root[u] = find(root[u]);
    return root[u];
}
ll merge(ll u,ll v) // 并查集合并节点 并返回判断是否可成环
{
	u = find(u);v = find(v);
    if(u == v) return 1;
    root[u] = v;
    return 0;
}
void dfs(ll u,ll tag) // u表示节点 tag即目标taget
{
    vis[u] = 1; // 记录搜索过的节点
    ans.push_back(u); // 记录路径
    if(u == tag) // 如果找到目标
    {
        cout << ans.size() << "\n"; // 输出路径大小
        for(ll &x :ans) cout << x << " "; // 输出路径
        cout << "\n";
        return; // 终止递归
    }
    for(ll &v :e[u])
        if(!vis[v]) dfs(v,tag); // 若此节点未被搜索过继续搜索
    ans.pop_back(); // 向上回溯时 将存储在路径的节点删除
}
void solve()
{  
    // input ans pre_work
    cin >> n >> m; 
    ans.clear();
    for(ll i = 1;i <= n;++ i) root[i] = i,e[i].clear(),vis[i] = 0;
    for(ll i = 1;i <= m;++ i)
    {
        ll u,v,w;cin >> u >> v >> w;
        a[i] = {u,v,w};
    }
    // 按边权从大到小排序 保证最后得到的边权是最小的
    sort(a + 1,a + m + 1,[](edge x, edge y){return x.w > y.w;});
    ll mn = 1e9,id = 0; // mn表示最小边权 id表示相应编号
    for(ll i = 1;i <= m;++ i)
    {
        if(merge(a[i].u,a[i].v) && a[i].w < mn) // 如果此边的两节点已经在同一集合里 并且· 该边权更小
        {
            mn = a[i].w,id = i;
        }
    }
    // 建图
    for(ll i = 1;i <= m;++ i) 
    {
        if(i == id) continue; // 不存最小的边权的节点 不然路径会找错 抄近路啦
        ll u = a[i].u,v = a[i].v;
        e[u].push_back(v);
        e[v].push_back(u);
    }
    cout << mn << " "; // 输出找到的循环中最小的边权
    dfs(a[id].u,a[id].v);
}   

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int T = 1;cin >> T;
    while(T --) solve();
    return 0;
}
```

