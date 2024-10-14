[B. Kar Salesman](https://codeforces.com/contest/2022/problem/B)

### 题意

有汽车种数 $n$，第 $i-th$ 种汽车的数量为 $a[i]$，推销员最多向一位客户推销 $r$ 辆不同种汽车，问**至少**需要几位客户才能把汽车卖完？

### 题解

答案只可能有两种情况：
- 其中最多的数量
- 若答案超出这个，则一定是每次选择 $r$ 个不同种的车，直到结束，也就是 $\lceil sum / x \rceil$  

##### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 5e5 + 9;
ll a[N];
void solve()
{
	ll n, x;cin >> n >> x;
	ll sum = 0,maxn = 0;
	for(int i = 1;i <= n;i ++) cin >> a[i],maxn = max(a[i],maxn),sum += a[i];
	
	cout << max(maxn,(sum + x - 1) / x) << '\n';
	
}

int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();
	return 0;
}
```