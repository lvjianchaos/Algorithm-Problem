**比赛场次 ** [Codeforces Round 957 (Div. 3)](https://codeforces.com/contest/1992)

**比赛题目** E. Novice's Mistake

<!--more-->

3 seconds / 256 megabytes

standard input / standard output

## 题目

K1o0n 的第一个编程问题是这样的"Noobish_Monk有 n 个朋友。
$$
(1 \le n \le 100)
$$
 个朋友。每个朋友都送给他 a 个苹果作为生日礼物。 
$$
(1 \le a \le 10000)
$$
 个苹果作为生日礼物。收到这样的礼物，诺比什/蒙克很高兴，他回赠了 b 个苹果。 
$$
(1 \le b \le \min(10000, a \cdot n))
$$
 个苹果给他的朋友们。诺比西和尚还剩下多少个苹果？"

K1o0n 写了一个解决方案，但不小心把 n 的值看成了字符串，所以 
$$
n \cdot a - b
$$
 的值的计算方法不同。具体来说

- 当用字符串 n 乘以整数 a 时，他将得到字符串 
  $$
  s=\underbrace{n + n + \dots + n + n}_{a\ \text{times}}
  $$
  
- 从字符串 s 中减去整数 b 时，将删除最后的 b 个字符。如果 b 大于或等于字符串 s 的长度，则字符串 s 将变为空。

了解到这一点后，ErnKor 开始关注在给定的 n 中，有多少对 (a, b) 满足问题的约束条件，而 K1o0n 的解给出了正确答案。

"解法给出了正确答案 "意味着它输出了一个**非空**字符串，这个字符串转换成整数后等于正确答案，即 
$$
n \cdot a - b
$$
 的值。

## 输入

第一行包含一个整数 t ( 1≤t≤100 ) - 测试用例数。

对于每个测试用例，单行输入包含一个整数 n ( 1≤n≤100 )。

保证在所有测试用例中， n 都是不同的。

## 输出

对于每个测试用例，按以下格式输出答案：

第一行，输出整数 x 。- 给出的 n 中的错误测试次数。

在接下来的 x 行中，输出两个整数 ai 和 bi 。

## 样例

**输入**

> 3
> 2
> 3
> 10

**输出**

> 3
> 20 18 
> 219 216 
> 2218 2214 
> 1
> 165 162 
> 1
> 1262 2519 

## 解释

在第一个例子中， a = 20 、 b = 18 是合适的，因为
$$
" \text{2} " \cdot 20 - 18 = " \text{22222222222222222222} " - 18 = 22 = 2 \cdot 20 - 18
$$


## 题解

题意

给你一个n，找出所有的a，b满足(字符串)a*n - b位后缀 == (整数)a * n - b

思路

##### 【暴力 枚举】

我们首先分析题意，如果不使用特殊的操作，那么a·n - b的范围是严格小于1e6的，位数是<=6

而若使用的话，a*n - b 就是相当于 a * len(n) - b 位数最高是 100 * 1e4的，这二者差距是巨大的

我们尝试枚举a的值，而去限定b的范围去枚举b，得出答案

我们利用长度(位数)相同来限制b的范围，则 a * len(n) - b <= 6即：b >= a * len(n)，又不能使字符串为空，则b<a*len(n)

故这样a的量级是1e4，b的量级是常数7，这是合理的限制

具体看代码实现

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int n,dig[5];
struct _
{
	ll a,b;
};
vector<_> ans;

ll len(ll x)
{
	if(x == 0) return 1;
	ll res = 0;
	while(x)
	{
		x /= 10;
		res ++;
	}
	return res;
}

bool check(ll a,ll b)
{
	/// cout << len(n * a - b) << ' ' << len(n) * a - b << '\n';
	if(len(n * a - b) != len(n) * a - b) return false;
	ll m = 0;
	for(int i = 0;i < len(n) * a - b;i ++)
		m = m * 10 + dig[i % len(n)];
	return m == n * a - b;
}

void solve()
{
	ans.clear();
	cin >> n;
	int tmp = n,cnt = 0;
	while(tmp)
	{
		dig[cnt ++] = tmp % 10;
		tmp /= 10;
	}
	reverse(dig, dig + cnt);
	for(ll a = 1;a <= 10000;a ++)
	{
		ll lim = min(10000ll,min(a*n,len(n)*a));
		for(ll b = lim;b;b --)
			if(len(n) * a - b > 6) break;	
			else
				if(check(a,b))
					ans.push_back({a,b});
	}
	cout << ans.size() << '\n';
	for(auto &i : ans)
		cout << i.a << ' ' << i.b << '\n'; 
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();
	return 0;
}
```

