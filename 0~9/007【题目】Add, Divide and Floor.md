**比赛场次** [Educational Codeforces Round 158 (Rated for Div. 2)](https://codeforces.com/contest/1901)

**比赛题目** C. Add, Divide and Floor

<!--more-->

2 seconds / 256 megabytes

standard input / standard output

## 题目

You are given an integer array a1,a2,…,an(0≤ai≤1e9). In one operation, you can choose an integer x (0≤x≤1e18) and replace ai with ⌊(ai+x)/2⌋ (⌊y⌋ denotes rounding y down to the nearest integer) for all i from 1 to n. Pay attention to the fact that all elements of the array are affected on each operation.

Print the smallest number of operations required to make all elements of the array equal.

If the number of operations is less than or equal to n, then print the chosen x for each operation. If there are multiple answers, print any of them.

## 输入

The first line contains a single integer t (1≤t≤1e4) — the number of testcases.

The first line of each testcase contains a single integer n (1≤n≤2e5).

The second line contains n integers a1,a2,…,an (0≤ai≤1e9).

The sum of n over all testcases doesn't exceed 2e5.

## 输出

For each testcase, print the smallest number of operations required to make all elements of the array equal.

If the number of operations is less than or equal to n, then print the chosen x for each operation in the next line. If there are multiple answers, print any of them.

## 样例

**输入**

> 4
> 1
> 10
> 2
> 4 6
> 6
> 2 1 2 1 2 1
> 2
> 0 32

**输出**

> 0
> 2
> 2 5
> 1
> 1
> 6

## 解释

In the first testcase, all elements are already equal, so 0 operations are required. It doesn't matter if you print an empty line afterwards or not.

In the second testcase, you can't make less than 2 operations. There are multiple answers, let's consider the answer sequence [2,5]. After applying an operation with x=2, the array becomes [⌊(4+2)/2⌋,⌊(6+2)/2⌋]=[3,4]. After applying an operation with x=5 after that, the array becomes [⌊(3+5)/2⌋,⌊(4+5)/2⌋]=[4,4]. Both elements are the same, so we are done.

In the last testcase, you can't make less than 6 operations. Since 6 is greater than n, you don't have to print them. One possible answer sequence is [0,0,0,0,0,0]. We are just dividing the second element by 2 every time and not changing the first element.

## 题解

##### 题意

给你一组数，你将进行下列操作：选一个数x，数组中每个元素+x/2向下取整。问：使数组中**所有元素相等的操作次数**是多少？

##### 思路

##### 【数学 贪心】

易得我们只**要将最小数和最大数相等**，那么其他的都会相等，不严谨证明：在使最小数和最大数相等的过程中，中间的数一定会变成最小数或最大数的一个再继续下去。那么我们该如何操作呢？要使操作数最小我们只需要选取x=0或者1。为什么？因为这样不会变成无限，最终的情况也是都为0了。

好，那么什么时候选1，什么时候选0呢？我们只需要考虑两个数：最大数和最小数。

* 若它们都为偶数或奇数，选0，这样相差减少最多，且自身更接近零；

* 若最大数为奇数，最小数为偶数，选0，这样奇数向下取整，比选取1要优越1个；

* 若最小数为奇数，最大数为偶数，选取1。

  据此，进行暴力。

**注意**：若数组一开始就为相等，特判一下。

（这是证明：

设a<b，那么同时对a b进行加和除的操作只会变成a<=b,

所以问题变成：给n个数，希望最大值和最小值相等，因为最大值和最小值相等，根据上面这个条件就能得出所有值就都相等。

最大值最小值相等本质就是差为0，容易发现差是偶数的时候/2操作差会小一倍，差是奇数的时候，如果最大值是偶数差会向上取整，反之就是向下去整，我们肯定是希望尽可能少的步骤完成，也就是尽可能向下去整，

然后给的这个加的操作不会改变最大值和最小值的差，但是会改变最大值的奇偶性，所以证毕。）

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
ll a[N];

void solve()
{
	int n;cin >> n;
	for(int i = 1;i <= n;i ++) cin >> a[i];
    
	sort(a + 1,a + 1 + n);
    
	int cnt = 0;
	ll p = a[1];
    ll q = a[n];
    ll ans[N];
    
	if(p == q)
	{
		cout << 0 << '\n';
		return;
	}
    
	while(++ cnt)
	{
		if(p % 2 == 1 && q % 2 == 0)
		{
			p = (p + 1) / 2;
			q = (q + 1) / 2;
			ans[cnt] = 1;
		}
		else
		{
			p /= 2;
			q /= 2;
			ans[cnt] = 0;
		}
		
		if(p == q)
		{
			cout << cnt << '\n';
			if(cnt <= n)
			{
				for(int i = 1;i <= cnt;i ++)
				{
					cout << ans[i] << " \n"[i == cnt];
				}
			}
			break;
		}
	}
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

