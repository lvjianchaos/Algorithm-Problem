$time\ limit\ per\ test:\ 1\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$ 
$output:\ standard\ output$

#### **题目**

Index 在生日派对上又收到了一个整数数组 $a_1, a_2, \ldots, a_n$ ，她决定对其执行一些操作。

从形式上看，她要依次执行的操作有 $m$ 个。每个操作都属于两种类型之一：

- $\texttt{+ l r}$ .给定两个整数 $l$ 和 $r$ ，对于所有 $1 \leq i \leq n$ 中的 $l \leq a_i \leq r$ ，设 $a_i := a_i + 1$ .
 - $\texttt{- l r}$ . 已知两个整数 $l$ 和 $r$ ，对于所有 $1 \leq i \leq n$ ，已知 $l \leq a_i \leq r$ ，设 $a_i := a_i - 1$ 。

例如，如果初始数组为 $a = [7, 1, 3, 4, 3]$ ，在执行运算 $\texttt{+} \space 2 \space 4$ 之后，数组为 $a = [7, 1, 4, 5, 4]$ 。然后，在执行操作 $\texttt{-} \space 1 \space 10$ 之后，数组 $a = [6, 0, 3, 4, 3]$ 。

Index 对数组 $a$ 中的最大值很好奇。请帮助她找到每次 $m$ 操作后的最大值。


#### **输入**

每个测试由多个测试用例组成。第一行包含一个整数 $t$ ( $1 \leq t \leq 2 \cdot 10^4$ ) - 测试用例数。测试用例说明如下。

每个测试用例的第一行包含两个整数 $n$ 和 $m$ ( $1 \leq n \leq 10^5$ , $1 \leq m \leq 10^5$ )--数组的长度和操作次数。

每个测试用例的第二行包含 $n$ 个整数 $a_1, a_2, \ldots, a_n$ ( $1 \leq a_i \leq 10^9$ ) - 初始数组 $a$ 。

然后是 $m$ 行，每行对应一个操作，格式如下： $\texttt{c l r}$ （ $c \in \\{\texttt +, \texttt -\\}$ 、 $l$ 和 $r$ 为整数， $1 \leq l \leq r \leq 10^9$ 为操作说明。

请注意， $a_i$ 元素**在某些运算后可能不满足** $1\le a_i\le 10^9$ ，如示例所示。

保证所有测试用例中 $n$ 的总和不超过 $10^5$ ，所有测试用例中 $m$ 的总和不超过 $10^5$ 。

#### **输出**

对于每个测试用例，输出一行包含 $m$ 个整数的数据，其中 $i$ 个整数描述了数组在 $i$ 次操作后的最大值。

#### **题解**：

###### 【思维】

第一眼，区间更改+维护最值问题，思之 **线段树**；
后来才发现并非区间修改，而是在$[l,r]$内大小的数修改，发现其在 B-题 之位，应有简易方法解之。

分类讨论：
- 区间不包含初最大值：
	- 得出最大值无影响
- 区间    包含初最大值：
	- 最大值 ++ 或 --
发现答案只与最大值有关联。

```c++
# include <bits/stdc++.h>
# define is_between_lr l <= maxn && maxn <= r
using namespace std;
using ll = long long;
const int N = 2e5 + 9;
vector<ll> a;
void solve()
{
	a.clear();
	int n,m,x;cin >> n >> m;
	for(int i = 0;i < n;i ++) cin >> x,a.push_back(x);
	ll maxn = *max_element(a.begin(),a.end());
	while(m --)
	{
		int l,r;char op;
		cin >> op >> l >> r;
		if(op == '+' && is_between_lr) maxn ++;
		else if(op == '-' && is_between_lr) maxn --;
		cout << maxn << ' ';
	}
	cout << '\n';
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();
	return 0;
}
```