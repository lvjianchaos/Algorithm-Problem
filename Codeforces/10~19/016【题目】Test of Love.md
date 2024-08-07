**比赛场次 ** [Codeforces Round 957 (Div. 3)](https://codeforces.com/contest/1992)

**比赛题目** D. Test of Love

<!--more-->

2 seconds / 256 megabytes

standard input / standard output

## 题目

恩科尔愿意为朱伦做任何事，甚至愿意游过鳄鱼出没的沼泽。我们决定测试一下这份爱。恩科尔必须游过一条宽 1 米、长 n 米的河流。

河水非常冷。因此，(即从 00 游到 n+1 的整个过程)恩科尔在水中游泳的总长度不超过 k 米。为了人性化起见，我们不仅在河里加入了鳄鱼，还加入了可以让他跳上去的圆木。我们的测试如下

一开始，恩科尔在左岸，需要到达右岸。它们分别位于 00 和 n+1 米处。河道可以表示为 n 个河段，每个河段的长度为 11 米。每个河段都包含原木 "L"、鳄鱼 "C "或水 "W"。恩科的移动方式如下

- 如果他在水面上(即岸上或原木上)，他可以向前跳跃不超过 m ( 1≤m≤10 ) 米(他可以在岸上、原木上或水中跳跃)。
- 如果他在水中，他只能游到下一个河段(如果他在 n 米处，则只能游到岸边）。
- 无论如何，恩科尔 都不能在有鳄鱼的河段登陆。

确定恩科尔能否到达右岸。

## 输入

第一行包含一个整数 t ( 1≤t≤1e4 )--测试用例数。

每个测试用例的第一行包含三个数字 n,m,k ( 0≤k≤2⋅1e5 , 1≤n≤2⋅1e5 , 1≤m≤10 )--河流的长度、恩科尔可以跳跃的距离，以及恩科尔在不受冻的情况下可以游泳的米数。

每个测试用例的第二行包含长度为 n 的字符串 a 。 ai 表示位于 i-th 米处的物体。( ai∈{ 'W','C','L' } )

保证所有测试用例的 n 之和不超过 2⋅1e5 。

## 输出

对于每个测试用例，如果 ErnKor 可以通过测试，则输出 "YES"，否则输出 "NO"。

您可以用任何大小写(大写或小写)输出答案。例如，字符串 "yEs"、"yes"、"Yes "和 "YES "将被识别为肯定回答。

## 样例

**输入**

> 6
> 6 2 0
> LWLLLW
> 6 1 1
> LWLLLL
> 6 1 1
> LWLLWL
> 6 2 15
> LWLLCC
> 6 10 0
> CCCCCC
> 6 6 1
> WCCCCW

**输出**

> YES
> YES
> NO
> NO
> YES
> YES

## 解释

让我们举例说明：

- 第一个例子：我们从岸边跳到第一根圆木( 0→1 )，从第一根圆木跳到第二根圆木( 1→3 )，从第二根圆木跳到第四根圆木( 3→5 )，再从最后一根圆木跳到岸边( 5→7 )。因此，我们得到 0→1→3→5→7 。由于我们没有遇到鳄鱼，而且游了不超过 k 米，所以答案是 "是"。
- 第二个例子 0→1 ，我们从第一根圆木( 1→2 )跳入水中，游一格到圆木( 2→3 )， 3→4→5→6→7 。由于我们没有遇到鳄鱼，而且游了不超过 k 米，所以答案是 "是"。
- 在第三个例子中，ErnKor 需要游过两个单元格 "W"，但只能游过一个单元格。因此，答案是 "否"。
- 第六个例子：我们从岸上跳入水中( 0→6 )，在水中游动一个单元格( 6⇝7 )。由于我们没有遇到鳄鱼，而且游了不超过 k 米，所以答案是 "是"。

## 题解

##### 【贪心】

如果可以跳，我们希望尽可能跳到岸边。如果不能跳到对岸，我们要跳到前面的任何一根圆木上，以便以后从圆木上跳下。如果不能跳到圆木，我们就尽可能跳得远，以避开鳄鱼，并尽可能少游泳。

具体看代码

##### 【dp】

dp[i] 表示 到达 i-th 所需游的最小米数

更新规则

![image-20240716130944895](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240716130944895.png)

如何理解dp？

思考	最原始的dp应该如何实现？

我们这样设计：bool f(i,k) ==》我到达i游了k米这个状态能否存在。

如果i位置是鳄鱼的话则是false；

否则我们考虑他是如何到达 i：

1. 从i-1游过来：f(i - 1,k - 1) 
2. 跳过来，枚举 j（1.j 是木头 2. i - j <= m) f(j,k)

最后答案就是看f(n + 1,k) 是否存在

但是这样时间复杂度就特别大，由于n，k都是2e5的

那么我们对于状态数很大的dp应该如何改进呢？

一般来讲都是把某一维状态放在值里，也就是说把k这个状态放进值里。

**f(i) ==> 到达i所需最少的游泳米数**，枚举到i的两种情况

1. 游泳： f(i - 1) + 1
2. 跳：枚举 j (……) ，f(j)

最终答案就是看f(n + 1) <= k。 

## 代码

【greedy】

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll N = 2e5 + 9;
string a;

void solve()
{
	ll i = 0,n,m,k,cnt = 0;cin >> n >> m >> k;
	cin >> a;
	a = "L" + a + "L"; // 预处理，方便后续判断是否上岸
	while(i <= n)
	{
		/*
			此位置为'L'时，
				如果能跳过到岸，就跳，输出"YES"
				否则，找出下一个'L'，
					如果找到，就跳到下一个'L'上，然后，进入下一循环，重复上述操作
					否则，尽可能跳得远，跳到i+m位置
						如果该位置为'W',则进入a[i] = 'W'时的操作
						若为'C',则输出"NO"
		*/
		if(a[i] == 'L') 
		{
			if(i + m > n)
			{
				cout << "YES\n";
				return;
			}
			int f = 0;
			for(ll j = i + m;j >= i + 1;j --)
			{
				if(a[j] == 'L') 
				{
					i = j;
					f = 1;
					break;
				}
			}
			if(f) continue;
			if(a[i + m] == 'C') 
			{
				cout << "NO\n";
				return;
			}
			if(a[i + m] == 'W') 
			{
				i += m;
				continue;
			}
		}
		/*
			此位置为'W'时，向前游
				若位置里为'W',则总游泳米数++,判断是否超出耐受米数，超出则输出"NO"
				若位置里为'C'，则输出"NO"
				若位置里为'L',则上去，进行在'L'位置时的操作
		*/
		if(a[i] == 'W')
		{
			for(int j = i;;j ++)
			{
				if(a[j] == 'L')
				{
					i = j;
					break;
				}
				else if(a[j] == 'C')
				{
					cout << "NO\n";
					return;
				}
				else 
				{
					cnt ++;
					if(cnt > k) 
					{
						cout << "NO\n";
						return;
					}
				}
			}
		}
	}
	
	cout << "YES\n"; // 主循环结束，说明期间并未冻伤且遭遇鳄鱼
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	
	while(t --)
	{
		solve();
	}
	
	return 0;
}
```

【dp】

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 2e5 + 9;
int dp[N];
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --)
	{
		ll n,m,k;cin >> n >> m >> k;
		string s;cin >> s;
		s = "L"+s+"L";
		for(int i = 1;i <= n + 1;i ++) 
		{
			dp[i] = 1e9; // 赋初值
			if(s[i] != 'C')
			{
				if(s[i - 1] == 'W') dp[i] = dp[i-1] + 1; // 游
				for(int j = 1;j <= m;j ++) // 跳
				{
					if(i - j >= 0 && s[i-j] == 'L')
					{
						dp[i] = min(dp[i],dp[i-j]);
					}
				}
			}
		}
		cout << (dp[n+1] <= k ? "YES\n" : "NO\n");
	}
	return 0;
}
```

