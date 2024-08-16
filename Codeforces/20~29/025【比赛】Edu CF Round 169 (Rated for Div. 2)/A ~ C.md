### A - Closest Point

$standard \ input/output$
$2\ s, 512\ MB$

给你一组点 $x[n]$ ($2\le n\le 40$ $1\le x_1 < x_2 < ... <x_n\le 100$)，他们排列在一条直线上。问你是否能添加一个整数的不同于现有的点，使得它成为与集合中每个点最近的点。

###### 【基础】【思维】
1. 只要点数大于 $2$ ，就一定不存在这样的点——不妨设有 $a,b,c$ 三点，所添加的点 $x$ 有 $4$ 种情况：$x,a,b,c$  $a,x,b,c$  $a,b,x,c$  $a,b,c,x$  ，它总会使 存在 至少一个原有的点 在 它 和 另一个原有点 之间，这时 它 和 另一个点 一定不是最近的。
2. 而当点数等于 $2$ 时，若两点相邻，也不会存在这样的点；否则存在，在两数之间。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;

const ll INF = 0x3f3f3f3f;
const ll MOD = 1e9 + 7;
const ll N = 2e5 + 9;

void solve()
{
	int n;cin >> n;
	int a[n];
	bool fg = true;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	if(n > 2) fg = false;
	else if(abs(a[1] - a[2]) == 1) fg = false;
	cout << (fg ? "YES" : "NO") << '\n';
}

int main()
{
    ios::sync_with_stdio(0);cin.tie(0),cout.tie(0);
    int _ = 1; cin >> _;
	while(_ --) solve();
    return 0;
}
```
### B - Game with Doors

$standard\ input/output$
$2\ s, 256\ MB$

$100$ 个房间排成一排，其间有 $99$ 个门； $i-th$ 门连接 $i$ 和 $i+1$ 两个房间。初始，所有门都没上锁。
我们说房间 $y$ 和房间 $x$ 可到达，若 $x$ 和 $y$ 之间所有的门都没上锁。

现在 $Alice$ 在房间 $[l,r]$ 之间的某个房间； $Bob$ 在房间 $[L,R]$之间的某个房间。若要使 $Alice$ 和 $Bob$ 之间无法接触，即他们所在房间不可互相到达，你至少要锁多少个房门？

###### 【分类讨论】

画图分类讨论即可。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;

const ll INF = 0x3f3f3f3f;
const ll MOD = 1e9 + 7;
const ll N = 2e5 + 9;

void solve()
{
	int ans = 0;
	int l1,r1;cin >> l1 >> r1;
	int l2,r2;cin >> l2 >> r2;
	if(l1 > l2) swap(l1,l2),swap(r1,r2);
	
	if(r1 < l2) ans = 1; // 若二者的区间不相交
	else
	{
		if(l1 == l2) // 若二者左端点相同
		{
			if(r1 == r2) ans = r1 - l1; // 同时右端点相同
			else ans = min(r1,r2) - l1 + 1; // 或不同
		}
		else // 二者左端点不同
		{
			if(r1 == r2) ans = r1 - l2 + 1; // 二者右端点相同
			else ans = min(r1,r2) - l2 + 2; // 右端点不同
		}
	}
	cout << ans << '\n';
}

int main()
{
    ios::sync_with_stdio(0);cin.tie(0),cout.tie(0);
    int _ = 1; cin >> _;
	while(_ --) solve();
    return 0;
}
```


### C - Splitting Items

$standard\ input/output$
$2\ s, 256\ MB$

$Alice$ 和 $Bob$ 玩一个游戏：有 $n$ 件物品，$i-th$ 物品有价值 $a[i]$ 。每个人一回合只能拿一件物品，且二人都想最大化自己所拿物品的总价值。假设 $A$ 是爱丽丝拿走物品的总成本， $B$ 是鲍勃拿走物品的总成本。若二者都按照最优方式博弈，请你输出 $A-B$。
但是作为后手的补偿，$Bob$ 可以增加任意物品的价值，且增加的总额必须 $\le k$。

($2 \le n \le 2 \cdot 10^5$; $0 \le k \le 10^9$) ($1 \le a_i \le 10^9$) 
保证所有测试用例的 $n$ 的总和不超过 $2 \cdot 10^5$。
###### 【博弈论】【贪心】

已知 $Alice$ 先手，所以每次都会拿现有最大值的物品 $a$，而 $Bob$ 只能拿第二大的物品 $b$ ($a\ge b$) 。所以 $Bob$ 只能尽可能的补偿自己来增加自己的物品的价值 $b$，但是所加价值后又不能超过 $a$ ，否则$Alice$ 会拿原本 $Bob$ 拿的物品，比起将所加物品价值提高到等于$a$，即 $b + add = a$ ，要更劣且无谓消耗了$k$。所以我们的目标就是每次将 $b$ 尽可能的 $+$ 到 $a$ 。  

直接模拟过程
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;

const ll INF = 0x3f3f3f3f;
const ll MOD = 1e9 + 7;
const ll N = 2e5 + 9;

ll a[N];

void solve()
{
	int n,k;cin >> n >> k;
	ll sum_alice = 0,sum_bob = 0;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	sort(a + 1,a + 1 + n,greater<ll>());
	for(int i = 1;i <= n;i ++) 
	{
		if(i&1) sum_alice += a[i];
		else
		{
			if(a[i-1] - a[i] <= k) k -= a[i-1] - a[i], sum_bob += a[i-1];
			else sum_bob += a[i] + k, k = 0;
		}
	}
	cout << sum_alice - sum_bob << '\n'; 
}

int main()
{
    ios::sync_with_stdio(0);cin.tie(0),cout.tie(0);
    int _ = 1; cin >> _;
	while(_ --) solve();
    return 0;
}
```
改进
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;

const ll INF = 0x3f3f3f3f;
const ll MOD = 1e9 + 7;
const ll N = 2e5 + 9;

ll a[N];

void solve()
{
	int n,k;cin >> n >> k;
	ll sum_alice = 0,sum_bob = 0;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	sort(a + 1,a + 1 + n,greater<ll>());
	for(int i = 1;i <= n;i ++) 
	{
		if(i&1) sum_alice += a[i];
		else sum_bob += a[i];
	}
	ll ans = sum_alice - sum_bob - k;
	ans = max(0ll,ans);
	cout << ans << '\n'; 
}

int main()
{
    ios::sync_with_stdio(0);cin.tie(0),cout.tie(0);
    int _ = 1; cin >> _;
	while(_ --) solve();
    return 0;
}
```

小变化
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;

const ll INF = 0x3f3f3f3f;
const ll MOD = 1e9 + 7;
const ll N = 2e5 + 9;

ll a[N];

void solve()
{
	int n,k;cin >> n >> k;
	ll sum_alice = 0,sum_bob = 0;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	sort(a + 1,a + 1 + n,greater<ll>());
	for(int i = 1;i <= n - (n&1);i ++) 
	{
		if(i&1) sum_alice += a[i];
		else sum_bob += a[i];
	}
	ll ans = sum_alice - sum_bob - k;
	ans = max(0ll,ans);
	if(n&1) ans += a[n];
	cout << ans << '\n'; 
}

int main()
{
    ios::sync_with_stdio(0);cin.tie(0),cout.tie(0);
    int _ = 1; cin >> _;
	while(_ --) solve();
    return 0;
}
```