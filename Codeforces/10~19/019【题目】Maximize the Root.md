**比赛场次**	[Educational Codeforces Round 168 (Rated for Div. 2)](https://codeforces.com/contest/1997)

**比赛题目**	D. Maximize the Root

<!--more-->

3 seconds / 256 megabytes

standard input / standard output

## 题目

大意：

给你一个有根树，有n个节点，编号1~n，根节点为1。每个节点i都有一个权值a[i]。你可以进行以下操作：选取非叶子节点将其权值+1，并将该节点下的所有子孙的权值都-1，你要保证操作后所有权值都是非负的。请你计算出根节点的最大权值。

## 输入

The first line contains a single integer tt (1≤t≤1e4) — the number of test cases.

The first line of each test case contains a single integer n (2≤n≤2⋅1e5) — the number of vertices in the tree.

The second line contains nn integers a1,a2,…,an (0≤ai≤1e9) — the initial values written at vertices.

The third line contains n−1n−1 integers p2,p3,…,pn (1≤pi≤n), where pipi is the parent of the ii-th vertex in the tree. Vertex 1 is the root.

Additional constraint on the input: the sum of nn over all test cases doesn't exceed 2⋅1e5.

## 输出

对于每个测试用例，打印一个整数-使用上述操作写入根处的最大可能值。

## 样例

**输入**

> 3
> 4
> 0 1 0 2
> 1 1 3
> 2
> 3 0
> 1
> 5
> 2 5 3 9 6
> 3 1 5 2

**输出**

> 1
> 3
> 6

## 解释

In the first test case, the following sequence of operations is possible:

- perform the operation on v=3, then the values on the vertices will be [0,1,1,1];
- perform the operation on v=1, then the values on the vertices will be [1,0,0,0].

## 题解

##### 【树】【贪心】【dfs】

如何解决这样的问题呢？我们要保证不会出现负值的情况，模拟观察得到每次的+-操作是自下而上的，且越往下要消耗的权值会越多。显然，贪心——要尽可能的使根节点的权值最大，就要使从叶子节点向上的每个父节点都尽可能的大。从下向上考虑，使用+-操作要保证儿子不能小于父亲，这样可保证每次使用+-操作都不会产生负值。

这样下来，就好考虑的多了，若父亲比儿子小，父亲要取儿子和自己权值的平均值；若大，就取儿子的值；若有多个儿子，依次进行操作，取最小的。这样就保证了尽可能大的情况下不产生负值。

具体细节看代码。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 2e5 + 9;
ll a[N]; // 权值
ll f[N]; // 父节点
ll n; // 节点个数
vector<ll> v[N]; // 树

ll dfs(ll node)
{
	ll minn = 1e18;
	if(v[node].empty()) // 若为叶子节点
	{
		if(f[node] == 1) return a[node]; // 若父节点是根，不用作比较，直接返回儿子的权值即可，因为根要吸收儿子全部的权值
		if(a[f[node]] > a[node]) return a[node]; // 若父亲权值大，要保证儿子不出现负值，故儿子的权值向上放
		else return (a[f[node]] + a[node]) / 2; // 若儿子权值大，就返回二者平均值
	} 
	for(auto &i :v[node])
		minn = min(minn,dfs(i)); // 多个儿子时，取结果最小的，保证不出现负值
	// cout << node << ' ' << minn << '\n';
	if(node == 1) return a[1] + minn; // 若此时是根，返回根的权值+儿子全部权值
	if(f[node] == 1) return minn; // 若父亲是根，返回儿子权值，不做平均操作
	if(a[f[node]] > minn) return minn; // 与上同理
	else return (a[f[node]] + minn) / 2;
}
void solve()
{
	cin >> n;
	for(int i = 1;i <= n;i ++) 
	{
		cin >> a[i];
		v[i].clear();
		f[i] = 0;
	}
	for(int i = 2;i <= n;i ++)
	{
		ll x;cin >> x; 
		f[i] = x; // 父节点
		v[x].push_back(i); // 建树
	}
	
	ll ans = dfs(1);
	cout << ans << '\n'; // 输出答案
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();
	return 0;
}
```

