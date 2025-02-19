`time limit per test: 1 second`
`memory limit per test: 256 megabytes`
## 题意
有一个无限延伸的坐标系，和一个边长为 $m$ 的正方形，其左下角初始在 $(0,0)$ ；该正方形可以进行 $n$ 次移动并在坐标系中留下阴影，问你在$n$移动后所构成的阴影(不算初始位置)的边长是多少？其中，第 $i$ 次移动是向右移动 $x_i$ ,向上移动 $y_i$，$(0<x_i,y_i<m)$。

## 题解
#前缀和 #math
<img src="https://espresso.codeforces.com/d7b6a3718c5865a2a6d22b88f9e93e0dc2b77103.png" width="65%"/>
由 $x,y$ 的范围，我们知道，正方形的阴影都相交且位于第一象限；我们可以分别对 $x[i],y[i]$ 前缀和求出 $i$ 次移动后的坐标(左下角)；而阴影的边长就是n个正方形边长减去相交部分，即`4*n*m-2*(pre_x[i-1]+pre_y[i-1]-pre_x[i]-pre_y[i]+2*m)`。

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
ll a[N];

ll x[110],y[110],pre_x[110],pre_y[110];

void solve()
{
	int n,m;cin >> n >> m; 
	for(int i = 1;i <= n;i ++) cin >> x[i] >> y[i];
	for(int i = 1;i <= n;i ++)
	{
		pre_x[i] = pre_x[i-1] + x[i];
		pre_y[i] = pre_y[i-1] + y[i];
	}
	ll ans = 4*n*m;
	for(int i = 2;i <= n;i ++)
	{
		ans -= 2*(pre_x[i-1]+m-pre_x[i]+pre_y[i-1]+m-pre_y[i]);
	}
	cout << ans << '\n';
	return;
}

int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t = 1;cin >> t;
	while(t --) solve();
	return 0;
}
```


