**比赛场次**	[The 2021 China Collegiate Programming Contest (Harbin)](https://codeforces.com/gym/103447)

**比赛题目**	[J - Local Minimum](https://codeforces.com/gym/103447/problem/J)

<!--more-->

1 seconds / 512 megabytes

standard input / standard output

## 题目

大意：给你一个二位数组M[n] [m]，找出满足在它所在行和列都是最小的元素的个数，并输出。

## 输入

第一行包含两个整数 n,m(1≤n,m≤1000) ，表示给定矩阵的大小。

接下来的 n 行分别包含 m 个整数 Mi,j(1≤Mi,j≤1e6) ，表示给定矩阵。

## 输出

输出一行，包含一个整数，表示答案。

## 样例

**输入**

> 3 3
> 1 5 9
> 4 3 7
> 2 6 2

**输出**

> 3

## 解释

**注**

有 3 个条目 
$$
M_{1,1} = 1, M_{2,2} = 3, M_{3,3} = 2
$$
 满足约束条件。

## 题解

预处理出每行和每列的最小元素，在遍历数组进行比较，若与该行、该列最小元素都相等，答案++。最终输出答案。

##### 关键：

**找出每行每列最小元素**

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 1314;
ll a[N][N],min_lin[N],min_col[N];
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int n,m;cin >> n >> m;
	for(int i = 1;i <= n;i ++) min_lin[i] = 1234567;
	for(int i = 1;i <= m;i ++) min_col[i] = 1234567;
	for(int i = 1;i <= n;i ++) 
		for(int j = 1;j <= m;j ++)
		{
			cin >> a[i][j];
			min_lin[i] = min(min_lin[i],a[i][j]);
			min_col[j] = min(min_col[j],a[i][j]);
		} 
	int ans = 0;
	for(int i = 1;i <= n;i ++)
		for(int j = 1;j <= m;j ++)
			if(a[i][j] == min_lin[i] && a[i][j] == min_col[j]) ans ++;
		
	cout << ans << '\n';	
	return 0;
}
```

