**比赛场次 **[Codeforces Round 938 (Div. 3)](https://codeforces.com/contest/1955)

**比赛题目** D.Inaccurate Subsequence Search

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

Maxim has an array a of n integers and an array b of m integers (m≤n).

Maxim considers an array c of length m to be good if the elements of array c can be rearranged in such a way that at least k of them match the elements of array b.

For example, if b=[1,2,3,4] and k=3, then the arrays [4,1,2,3] and [2,3,4,5] are good (they can be reordered as follows: [1,2,3,4] and [5,2,3,4]), while the arrays [3,4,5,6]and [3,4,3,4]are not good.

Maxim wants to choose every subsegment of array a of length m as the elements of array c. Help Maxim count how many selected arrays will be good.

In other words, find the number of positions 1≤l≤n−m+1 such that the elements al,al+1,…,al+m−1 form a good array.

## 输入

The first line contains an integer t (1≤t≤1e4) — the number of test cases.

The first line of each test case contains three integers n, m, and k (1≤k≤m≤n≤2e5) — the number of elements in arrays a and b, the required number of matching elements.

The second line of each test case contains n integers a1,a2,…,an (1≤ai≤1e6) — the elements of array a. Elements of the array a are not necessarily unique.

The third line of each test case contains m integers b1,b2,…,bm (1≤bi≤1e6) — the elements of array b. Elements of the array b are not necessarily unique.

It is guaranteed that the sum of n over all test cases does not exceed 2e5. Similarly, it is guaranteed that the sum of m over all test cases does not exceed 2e5.

## 输出

对于每个测试用例，另起一行输出数组 a 中良好子段的数量。

## 样例

**输入**

> 5
> 7 4 2
> 4 1 2 3 4 5 6
> 1 2 3 4
> 7 4 3
> 4 1 2 3 4 5 6
> 1 2 3 4
> 7 4 4
> 4 1 2 3 4 5 6
> 1 2 3 4
> 11 5 3
> 9 9 2 2 10 9 7 6 3 6 3
> 6 9 7 8 10
> 4 1 1
> 4 1 5 6
> 6

**输出**

> 4
> 3
> 2
> 4
> 1

## 解释

**Note**

In the first example, all subsegments are good.

In the second example, good subsegments start at positions 1, 2, and 3.

In the third example, good subsegments start at positions 1 and  2.

## 题解

##### 题意

给你两个数组，在第一个数组中找出大小为m的子段，如果其中有k个与之匹配的元素，那么就是一个好数组，问总共有几个。

##### 思路

##### **【暴力 模拟 滑动窗口思想】**

**滑动窗口思想**，

每次将右端元素加入，判断是否匹配，若匹配将now++即当前窗口中元素的匹配数，然后记录下其存在于窗口中的个数；

如果窗口size大于m,将左端元素移出,并判断移除元素是否是匹配项且窗口中不存在；若是，则now--。

最终，判断窗口大小是否为m，并且窗口内匹配项数目now>=k，那么ans++.

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll N = 2e5 + 9;
const ll M = 1e6 + 9;
// 暴力 模拟 类似滑动窗口的思想
ll a[N],b[N],c[M],d[M];

void solve()
{
	int n,m,k;cin >> n >> m >> k;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	for(int i = 1;i <= m;i ++) cin >> b[i],c[b[i]] ++;
	int ans = 0,now = 0;// 指：答案 和 现在窗口内有多少匹配项
	for(int i = 1;i <= n;i ++)
	{
		if(d[a[i]] < c[a[i]]) now ++; // 若d[a[i]] < c[a[i]],则此窗口内多了一个与b匹配的元素
		d[a[i]] ++; // 将进来的元素记录
		if(i - m > 0) // 若i > m,则需要将最左端的移出
		{
			d[a[i - m]] --; // 最左端的移出
			if(d[a[i - m]] < c[a[i - m]]) now --; // 若移出的元素是相匹配的元素，并且窗口内不再有与其相等的元素，则now--
		}
		if(i >= m && now >= k)
		{
			ans ++;
		}
	}
	cout << ans << '\n';	
    // 桶要清零
	for(int i = 1;i <= n;i ++) d[a[i]] = 0;
	for(int i = 1;i <= m;i ++) c[b[i]] = 0;
	return;
}

int main(void)
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int _ = 1;cin >> _;
	while(_ --)
	{
		solve();
	}
	return 0;
}
```

