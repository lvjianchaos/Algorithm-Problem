**比赛场次**    [Educational Codeforces Round 169 (Rated for Div. 2)](https://codeforces.com/contest/2004)

**比赛题目**    D - Colored Portals

<!--more-->

2 seconds / 256 megabytes

standard input / standard output

## 题目

有 $n$ 个城市在一条直线上，编号 $1 - n$ ，每个城市有 $2$ 个颜色的门。门共有 $4$ 种颜色：红($R$)，黄{$Y$}，蓝($B$)，绿($G$)。若 $i-th$ 城市和 $j-th$ 城市的传送门颜色相同，你就可以在这两个城市之间互相传送，消耗 $abs(i - j)$ 枚金币。

给你 $q$ 个询问，计算从 $x$ 移动到 $y$ 的最小路费，若不能移动到，则输出 $-1$ 。

## 输入

第一行包含一个整数 $t$ （$1 \le t \le 10^4$） — 测试用例的数量。

每个测试用例的第一行包含两个整数 $n$ 和 $q$ （$1 \le n， q \le 2 \cdot 10^5$） — 分别是城市数量和查询数量。

第二行包含以下类型的 $n$ 字符串：BG、BR、BY、GR、GY 或 RY;其中 $i-th$ 描述了位于 $i-th$ 城市的门户;符号 B 表示城市中有一个蓝色门户，G — 绿色，R — 红色，Y — 黄色。

接下来的 $q$ 行的第 $j$ 行包含两个整数 $x_j$ 和 $y_j$ （$1 \le x_j， y_j \le n$） — 第 $j-th$ 查询的描述。

对输入的其他约束：

- 所有测试用例的 $n$ 之和不超过 $2 \cdot 10^5$;
- 所有测试用例的 $q$ 之和不超过 $2 \cdot 10^5$。

## 输出

对于每个查询，打印一个整数 — 从城市 $x$ 移动到城市 $y$ 的最低成本（如果不可能，则为 $-1$ ）。

## 样例

**输入**

> 2
> 4 5
> BR BR GY GR
> 1 2
> 3 1
> 4 4
> 1 4
> 4 2
> 2 1
> BG RY
> 1 2

**输出**

> 1
> 4
> 0
> 3
> 2
> -1

## 题解

###### 【二进制状态压缩】【模拟】【位运算】【set】

观察发现可以互相传送的城市只有 $2$ 种情况：
1. 两城市有相同颜色的门，直接传送；
2. 两城市没有相同颜色的门，需要一个中转站似的城市(与前一个有相同颜色，与后一个有相同颜色)，进行间接传送。
 主要方法：利用集合去压缩颜色的状态


## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;

const ll INF = 0x3f3f3f3f;
const ll MOD = 1e9 + 7;
const ll N = 2e5 + 9;

ll a[N];
int n,q;
set<int> se[16]; // 有该颜色状态 的 编号

void solve()
{
	cin >> n >> q;
	// 清空集合
	for(int i = 0;i < 16;i ++) se[i].clear();
	for(int i = 1;i <= n; i++)
	{
		string s;cin >> s;
		a[i] = 0;
		// 二进制压缩4种颜色的状态
		for(auto &c : s)
		{
			if(c == 'B') a[i] |= 1;
			if(c == 'G') a[i] |= 2;
			if(c == 'R') a[i] |= 4;
			if(c == 'Y') a[i] |= 8;
		}
		// 将编号插入对应的颜色状态
		se[a[i]].insert(i);
	}
	while(q --)
	{
		int x,y;cin >> x >> y;
		if(x > y) swap(x,y);
		int ans = 1e9;
		for(int i = 0;i < 16;i ++) 
		{
			// the se[i] has x' color and y' color 
			if((i&a[x]) && (i&a[y]))
			{
				// 二分得到在集合i下的 >= y 的中转站(可能为y)的迭代器 it
				auto it = se[i].lower_bound(y);
				// 若迭代器存在
				if(it != se[i].end()) ans = min(ans,2 * (*it) - x - y);
				// 若迭代器不是首位，it --，也就是 <y 且最接近y的迭代器
				if(it != se[i].begin())
				{
					-- it;	
					ans = min(ans,abs((*it) - x) + abs((*it) - y));
				}
			}
		}
		cout << (ans < 1e9 ? ans : -1) << '\n';
	}
}

int main()
{
    ios::sync_with_stdio(0);cin.tie(0),cout.tie(0);
    int _ = 1; cin >> _;
	while(_ --) solve();
    return 0;
}
```