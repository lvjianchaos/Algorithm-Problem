**比赛场次**    [Codeforces Round 966 (Div. 3)](https://codeforces.com/contest/2000)

**比赛题目**    F - Color Rows and Columns

<!--more-->

3 seconds / 256 megabytes

standard input / standard output

## 题目

You have $n$ rectangles, the $i$\-th of which has a width of $a_i$ and a height of $b_i$. 

You can perform the following operation an unlimited number of times: choose a rectangle and a cell in it, and then color it. 

Each time you completely color any row or column, you earn $1$ point. Your task is to score at least $k$ points with as few operations as possible. 

Suppose you have a rectangle with a width of $6$ and a height of $3$. You can score $4$ points by coloring all the cells in any $4$ columns, thus performing $12$ operations.

## 输入

The first line contains an integer $t$ ($1 \le t \le 100$) — the number of test cases. The following are the descriptions of the test cases. 

The first line of each test case description contains two integers $n$ and $k$ ($1 \le n \le 1000, 1 \le k \le 100$) — the number of rectangles in the case and the required number of points. 

The next $n$ lines contain the descriptions of the rectangles. The $i$\-th line contains two integers $a_i$ and $b_i$ ($1 \le a_i, b_i \le 100$) — the width and height of the $i$\-th rectangle. 

It is guaranteed that the sum of the values of $n$ across all test cases does not exceed $1000$.

## 输出

For each test case, output a single integer — the minimum number of operations required to score at least $k$ points. If it is impossible to score at least $k$ points, output \-1.

## 样例

**输入**

> 7
> 1 4
> 6 3
> 1 5
> 4 4
> 5 10
> 1 1
> 1 1
> 1 1
> 1 1
> 1 1
> 2 100
> 1 2
> 5 6
> 3 11
> 2 2
> 3 3
> 4 4
> 3 25
> 9 2
> 4 3
> 8 10
> 4 18
> 5 4
> 8 5
> 8 3
> 6 2


**输出**

> 12
> 14
> 5
> -1
> 17
> 80
> 35

## 题解

###### 【赛时思考：贪心】
	赛时的时候，想利用贪心的思想进行求解 ———— 按照边长短的矩形进行排序，将矩形涂成剩余正方形，再一行一列取填涂 ———— 可惜并不能过，可以观察最后一个样例。
###### 【dp】

我们思考若已知`f[x]`，那么`f[x + k] = f[x] + c[i][k]`
其中`c[i][k]`表示从第`i`个矩形中得到`k`所需操作数。
`设k = j + l, c[i][j + l] = min(c[i][j + l],j * b[i] + l * a[i] - j * l);`
其实就是暴力枚举，若已知长宽，即可知道所需的操作数。
暴力遍历即可，因为数据范围较小最终导致时间复杂度在 $O(n)$ 左右

确定状态：`f[i]` 得到分数`i`的操作数
确定转移：`f2[i + j] = min(f2[i + j],f1[i] + c[x][j])
确定范围：分数<=100；矩形数<=1000；边长均<=100； 
## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll inf = 0x3f3f3f3f3f;
const ll p = 1e9 + 7;
const int N = 1e3 + 9;

int a[N],b[N];
ll c[N][222];
ll f[110];

void solve()
{
	// input.
	int n,k;cin >> n >> k;
	for(int i = 1;i <= n;i ++) cin >> a[i] >> b[i];
	// initialize array c and f.
	for(int i = 0;i <= n;i ++)
		for(int j = 0;j <= a[i] + b[i];j ++)
			c[i][j] = inf;
	for(int i = 0;i <= k;i ++) f[i] = inf;
	// calculate array c[i][x]: the min_color_count of the i-th rectangle getting a score x.
	for(int i = 1;i <= n;i ++)
		for(int j = 0;j <= a[i];j ++)
			for(int l = 0;l <= b[i];l ++)
				c[i][j + l] = min(c[i][j + l],ll(j * b[i] + l * a[i] - j * l)); 
	// [dp]calculate array f[i]: the final min_color_count of getting score x.
	f[0] = 0;
	for(int i = 1;i <= n;i ++)
	{
		ll f_[k + 1];
		for(int i = 0;i <= k;i ++) f_[i] = inf;  // initialize
		for(int j = 0;j <= a[i] + b[i];j ++)
		{
			for(int l = 0;l <= k;l ++)
			{
				if(j + l > k) break;	// over the k_range
				if(f[l] >= inf) continue;    // if f[l] still no existing
				f_[j + l] = min(f_[j + l],f[l] + c[i][j]);    // update
			}
		}
		for(int i = 0;i <= k;i ++) f[i] = f_[i];    // copy
	}
	// ooutput
	cout << (f[k] >= inf ? -1 : f[k]) << '\n';
}

int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	// multicase
	int _;cin >> _;
	while(_ --) solve();
	return 0;
}
```