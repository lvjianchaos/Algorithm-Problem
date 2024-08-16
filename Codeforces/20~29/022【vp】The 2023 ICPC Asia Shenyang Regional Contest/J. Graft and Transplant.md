**比赛场次**    [The 2023 ICPC Asia Shenyang Regional Contest (The 2nd Universal Cup. Stage 13: Shenyang)](https://codeforces.com/gym/104869)

**比赛题目**    J

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

Alice 和 Bob 在玩一个游戏，有一颗有$n$个节点的树，编号从 $1$ 到 $n$。
可以进行如下操作：选择一组相邻的节点$(u,v)$，你可以将$u$的所有除了$v$的节点删去与$u$的连接，并连接$v$节点。 但是操作前后两树不得同构。
(同构：$V（S）$ 是树中的顶点集 $S$，$V（T）$ 是树中的顶点集 $T$。如果存在双射$f$，则树 $S$ 和树 $T$ 是同构的：$V（S） \rightarrow V（T）$，使得对于所有顶点对 $（u，v）$ 在 $V（S）$、$u$ 和 $v$ 中通过 $S$ 中的边连接，当且仅当顶点 $f（u）$ 和 $f（v）$ 由 $T$ 中的边连接。也就是说，$f（s）=t$ 表示顶点 $s$ 表示 $S$ 对应于顶点 $t$ 表示 $T$。)

在这种情况下，当一个玩家无法执行任何有效的操作时，另一个玩家将赢得游戏。假设 Alice 和 Bob 都足够聪明，并且会采取最佳策略来获胜，或者在无法获胜时阻止对手获胜，您需要预测获胜者。

## 输入

The first line contains an integer $n$ ($2 \le n \le 50$), denoting the number of vertices in the tree. Then $n-1$ lines follow, each of which contains two integers $u$ and $v$ ($1 \le u, v \le n)$, denoting an undirected edge connecting vertices $u$ and $v$. It is guaranteed that the given edges form a tree.

## 输出

Output "Alice" if Alice will win the game, "Bob" if Bob will win the game, or "Draw" if the game under the extra rule will still never end if both of them perform their operations optimally.

## 样例

**输入**

> 4
> 1 2
> 2 3
> 3 4


**输出**

> Alice

**输入**

> 4
> 1 2
> 1 3
> 1 4

**输出**

 > Bob

## 题解
##### 【思维】【树】

我们发现，每进行一次操作会使一个非叶子节点成为叶子节点，最后的不能操作的局面一定是叶子节点个数到达 $n-1$ 。

我们只需记录非叶子节点的个数即可。若为奇数则Bob获胜，为偶数则Alice获胜。另外需要注意在此之前，若只有 $0$ 个非叶子节点，Alice先手不能操作，故Bob胜。

关于记录是否为叶子节点，只需记录每个节点出现过的次数，若只出现一次，即为叶子，否则就是非叶子节点。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main(void)
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int n; cin >> n;
	vector<int> ind(n + 1);
	for (int i = 0; i < n - 1; i ++ )
	{
		int u, v;
		cin >> u >> v;
		ind[u] ++, ind[v] ++;
	}
	int cnt = 0;
	for (int i = 1; i <= n; i ++ )
		if (ind[i] != 1) cnt ++ ;
	if (cnt == 0 || cnt & 1) cout << "Bob\n";
	else cout << "Alice\n";
	return 0;
}
```