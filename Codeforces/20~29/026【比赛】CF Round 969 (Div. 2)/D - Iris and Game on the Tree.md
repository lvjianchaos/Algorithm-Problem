$time\ limit\ per\ test:\ 2\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$ 
$output:\ standard\ output$

**题意**

有一个树（根节点为1不被看作叶子），每个节点有值 0 或 1，用一个长为 n 的01字符串 s 描述从 1~n 的节点值。定义一个叶子的重为从根节点到此节点值构成的01串 10 的出现次数和 01 的差值。一棵树的重就是叶子的重之和。
此时，我们将字符串 s 中存在一些 ? ，Iris 和 Dora在其中填写1或0 进行博弈，Iris先手且尽可能使树的权重大，Dora反之。问这棵树的最终权重。

**输入**

每个测试由多个测试用例组成。第一行包含一个整数 $t$ ( $1 \leq t \leq 5\cdot 10^4$ ) - 测试用例数。测试用例说明如下。

每个测试用例的第一行都包含一个整数 $n$ ( $2 \leq n \leq 10^5$ ) - 树的顶点数。

接下来的 $n - 1$ 行分别包含两个整数 $u$ 和 $v$ ( $1 \leq u, v \leq n$ ) - 表示顶点 $u$ 和 $v$ 之间的一条边。

可以保证给定的边构成一棵树。

最后一行包含长度为 $n$ 的字符串 $s$ 。 $s$ 的 $i$ -th 字符代表顶点 $i$ 的值。可以保证 $s$ 只包含字符 $\mathtt{0}$ 、 $\mathtt{1}$ 和 $\mathtt{?}$ 。

保证所有测试用例中 $n$ 的总和不超过 $2\cdot 10^5$ 。

**输出**

对于每个测试用例，输出一个整数 - 树的最终得分。

**题解**
###### 【树】【博弈】【贪心】

一个显得的规律，对于每个节点的权值只有 0 或者 1：
- 根节点的值与叶节点的值相同的时候，权值为0；
- 根节点的值与叶节点的值不同的时候，权值为1。
原因：画图理解，01 和 10 互为逆反，有上升会有下降。

分类讨论：
- 根节点不为 "?" ，
	- 则Iris尽可能将 "?" => 与根节点相反的值，Dora反之，所得 ans = 此时原有的与根节点不同的叶节点数 + ? 叶节点数 / 2 向上取整（因为Iris先手）。
- 根节点    为 "?" ，
	- 则lris有两条路：
		- 先填根节点，依据叶节点中01的个数，填上个数多的相反的。然后就是与Dora填叶节点。ans = 叶节点0或者1（最多）的个数 +  ? 叶节点数 / 2 向下取整（因为Iris先手填了根，相当于Dora先手填尾）。
		- 如果有无关点个数为奇数（非根非叶），lris可以先填无关点，导致Dora进行先手操作选择填根与否，若他填根，则 ans = 叶节点0或者1（最少）的个数 + ? 叶节点数 / 2 向上取整；若他填叶子，肯定是填与最多的相反的值，这时与第一个无异。
		- 当然要取两个中的最大值。

```c++
# include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
int g[N];
string s;

void solve()
{
	int n;cin >> n;
	for(int i = 1;i <= n;i ++) g[i] = 0;
	for(int i = 1;i < n;i ++)
	{
		int u,v;
		cin >> u >> v;
		g[u] ++;g[v] ++;
	}
	cin >> s;s = " " + s;
	int a,b,c,r;a = b = c = r = 0;
	for(int i = 2;i <= n;i ++)
	{
		if(g[i] == 1)
		{
			if(s[i] == '0') a ++;
			else if(s[i] == '1') b ++;
			else c ++;
		}
		else
		{
			if(s[i] == '?') r ++;
		}
	}
	if(s[1] == '?')
	{
		int ans = max(a,b) + c / 2;
		if(r&1) ans = max(ans,min(a,b) + (c+1)/2);
		cout << ans << '\n';
	}
	else
	{
		int ans = 0;
		if(s[1] == '0') ans = b + (c + 1) / 2;
		else ans = a + (c + 1) / 2 ;
		cout << ans << '\n';
	}
	return;
}
int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}
```