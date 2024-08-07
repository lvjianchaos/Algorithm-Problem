**比赛场次 **	2024 Jiangsu Collegiate Programming Contest

**比赛题目**	I. Integer Reaction

<!--more-->

3 seconds / 1024 megabytes

standard input / standard output

## 题目

一列 n 个数, 每个数有两种颜色. 这些数按从左到右顺序依次进 入一个多重集合 S1, 如果 S1 中有与其不同颜色的数, 就选一个进 行反应, 将这两个数的和放入 S2 集合中, 这两个数均消失; 否则 添加到 S1 集合中, 不发生反应. 求 S2 中最小元素的可能最大值.

## 输入

第一行包含一个整数 n ( 2≤n≤1e5 )，代表整数个数。

第二行包含 n 个正整数 a1,a2,…,an ( 1≤ai≤1e8 )，代表整数序列。

第三行包含 n 个整数 c1,c2,…,cn ( ci∈{0,1} )。( ci∈{0,1} )，其中 ci 表示 i 个整数的颜色。

可以保证至少有一个 i 是 ci=0 ，至少有一个 j 是 cj=1 。

## 输出

输出一个整数，代表答案。

## 样例

**输入**

> 4
> 1 3 2 4
> 0 0 1 1

**输出**

> 5

**输入**

> 6
> 1 3 4 2 5 6
> 0 1 0 1 0 1

**输出**

> 4

**输入**

> 7
> 3 3 4 4 5 3 1
> 0 0 1 1 1 0 0

**输出**

> 7

## 题解

#### 【二分答案 贪心】

* 由于至少存在一次反应, S2 中最小元素一定存在最大值, 因此考虑**二分答案**. 则问题转化成一个判定问题, 即: <u>是否存在一种反应的方案, 使得 S2 中的最小元素不小于 x.</u> 

* 考虑**贪心**地进行反应. 维护多重集合 S1, 容易知道每时每刻 S1 中元素颜色均相同, 每当枚举到与 S1 中元素颜色不同的元素 y 时, 选择大于等于 x − y 的最小元素与其反应, 如果不存在这样的元素, 则判定不存在方案. 如果成功处理到最后, 则说明存在选择方案.
* 这样贪心的正确性在于: 由于要求 S2 中最小的元素不小于 x, 那么一定需要选择大于等于 x − y 的元素与其反应, 满足选择性质; 如果选择了大于等于 x − y 的元素, 我们总可以选择最小的一个, 使后续较小的 y 可以匹配到较大的元素, 如果交换这两次选择, 后续较小的 y 将可能无法匹配, 因此选择最小的一个具有优化子结构. 
* 时间复杂度: O(n log(n)log(max{ai})), 空间复杂度: O(n).

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 1e5 + 9;
ll a[N],c[N],n;
multiset<ll> s1;
bool check(ll x)
{
	s1.clear();
	int cnt0 = 0,cnt1 = 0;
	for(int i = 1;i <= n;i ++)
	{
		if(c[i] == 0)
		{
			if(cnt1)
			{
				cnt1 --;
				if(s1.lower_bound(x - a[i]) == s1.end()) return false;
				else s1.erase(s1.lower_bound(x - a[i]));
			}
			else
			{
				cnt0 ++;
				s1.insert(a[i]);
			}
		} 
		else 
		{
			if(cnt0)
			{
				cnt0 --;
				if(s1.lower_bound(x - a[i]) == s1.end()) return false;
				else s1.erase(s1.lower_bound(x - a[i]));
			}
			else
			{
				cnt1 ++;
				s1.insert(a[i]);
			}
		}
	}
	return true;
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	cin >> n;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	for(int i = 1;i <= n;i ++) cin >> c[i];
	ll l = 1,r = 3e8;
	while(l + 1 != r)
	{
		ll mid = ((l + r) >> 1);
		if(check(mid)) l = mid;
		else r = mid;
	}
	cout << l << '\n';
	return 0;
}
```

