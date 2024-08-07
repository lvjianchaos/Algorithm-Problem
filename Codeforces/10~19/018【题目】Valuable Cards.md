**比赛场次** [Codeforces Round 957 (Div. 3)](https://codeforces.com/contest/1992)

**比赛题目** F. Valuable Cards

<!--more-->

4 seconds / 256 megabytes

standard input / standard output

## 题目

In his favorite cafe Kmes once again wanted to try the herring under a fur coat. Previously, it would not have been difficult for him to do this, but the cafe recently introduced a new purchasing policy.

Now, in order to make a purchase, Kmes needs to solve the following problem: n cards with prices for different positions are laid out in front of him, on the i-th card there is an integer a_i, among these prices there is no whole positive integer x.

Kmes is asked to divide these cards into the minimum number of bad segments (so that each card belongs to exactly one segment). A segment is considered bad if it is impossible to select a subset of cards with a product equal to x. All segments, in which Kmes will divide the cards, must be bad.

Formally, the segment (l, r) is bad if ther

 such that
$$
l \le i_1, i_k \le r, and a_{i_1} \cdot a_{i_2} \ldots \cdot a_{i_k} = x.
$$
Help Kmes determine the minimum number of bad segments in order to enjoy his favorite dish.

## 输入

**输入**

第一行包含一个整数 
$$
t ( 1 \le t \le 10^3 )
$$
 - 测试用例数。

每组输入数据的第一行都包含 2 个整数 n 和 x 
$$
( 1 \le n \le 10^5, 2 \le x \le 10^5 )
$$
 - 分别是卡片的数量和整数。

每组输入数据的第二行包含 n 个整数 a ) - 纸牌的价格。

保证所有测试数据集的 n 之和不超过 10^5 。

## 输出

对于每组输入数据，输出坏线段的最少数量。

## 样例

**输入**

> 8
> 6 4
> 2 3 6 2 1 2
> 9 100000
> 50000 25000 12500 6250 3125 2 4 8 16
> 5 2
> 1 1 1 1 1
> 8 6
> 4 3 4 3 4 3 4 3
> 7 12
> 6 11 1 3 11 10 2
> 10 5
> 2 4 4 2 4 4 4 3 1 1
> 7 8
> 4 6 5 1 2 4 1
> 8 27
> 3 9 17 26 2 20 9 3

**输出**

> 3
> 2
> 1
> 1
> 2
> 1
> 3
> 3

## 题解

##### 【暴力】【贪心】【STL使用】

题意为：给你一数组，要求进行分段，使每一段都是坏的，坏的表示每一段中没有连续的序列使得相乘等于x，请输出最少的段数

进行思考，考虑到乘积，那么有作用的只有x的约数，因为别的数再怎么与其他数相乘都不能使其成为x；所以只对x的约束进行研究。再进行从前至后的遍历，将前面的所有乘积都存起来，再判断是否存在当前的x/a[i]在前面出现过，如果出现过，则分段；否则继续。

一般而言，我们会想到使用set来进行存储，如下：

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll N = 2e5 + 9;
int n, x, a[N];
unordered_set<int> S, T;

void solve()
{
	cin >> n >> x;
	
	S.clear();
	
	int res = 1;
	
	for (int i = 1; i <= n; ++ i ) {
		cin >> a[i];
		if (x % a[i]) continue;
		if (S.count(x / a[i])) {
			++ res;
			S.clear();
			S.insert(a[i]);
		}
		else {
			T.clear();
			for (int j : S) {
				if (j * a[i] <= x) T.insert(j * a[i]);
			}
			S.insert(a[i]);
			for (int j : T) S.insert(j);
		}
	}
	
	cout << res << '\n';
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();
	return 0;
}
```

但是会被卡常，这份代码的复杂度瓶颈在 for (int j : S) 里面。可以将 T 换成 vector。这样省掉了内部循环的一个 log。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const ll N = 2e5 + 9;
int n, x, a[N];
unordered_map<int, int> S;

void solve()
{
	cin >> n >> x;
	
	S.clear();
	
	int res = 1;
	
	for (int i = 1; i <= n; ++ i ) 
    {
		int ai;
		cin >> ai;
		if (x % ai) continue;
		if (S.count(x / ai)) 
        {
			++ res;
			S.clear();
		}
		else 
        {
			vector<int> T;
			for (auto t : S) 
            {
				int j = t.first;
				if (j * ai <= x) T.push_back(j * ai);
			}
			for (int j : T) S[j] ++ ;
		}
		S[ai] ++ ;
	}
	
	cout << res << '\n';
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();
	return 0;
}
```

