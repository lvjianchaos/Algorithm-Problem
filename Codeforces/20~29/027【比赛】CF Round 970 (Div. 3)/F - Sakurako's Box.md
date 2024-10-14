
$time\ limit\ per\ test:\ 2\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$
$output:\ standard\ output$

##### **Description**

Sakurako has a box with $n$ balls. Each ball has it's value. She wants to bet with her friend that if the friend randomly picks two balls from the box (it could be two distinct balls, but they may have the same value), the product of their values will be the same as the number that Sakurako guessed.

Since Sakurako has a PhD in probability, she knows that the best number to pick is [the expected value](http://tiny.cc/matozh_en), but she forgot how to calculate it. Help Sakurako and find the expected value of the product of two elements from the array.

It can be shown that the expected value has the form $\frac{P}{Q}$, where $P$ and $Q$ are non-negative integers, and $Q \ne 0$. Report the value of $P \cdot Q^{-1}(\bmod 10^9+7)$.

##### **Input**

The first line contains a single integer $t$ ($1\le t\le 10^4$)  — the number of test cases.

The first line of each test case contains a single integer $n$ ($2\le n\le 2\cdot 10^5$)  — the number of elements in the array.

The second line of each test case contains $n$ integers $a_1, a_2, \dots, a_n$ ($0\le a_i\le 10^9$)  — the elements of the array.

It is guaranteed that the sum of $n$ across all test cases does not exceed $2\cdot 10^5$.

##### **Output**

For each test case, output the value of $P \cdot Q^{-1}(\bmod 10^9+7)$.

##### **Solution**

【后缀和】【乘法逆元】【数学】

总共有 $C_n^2$ 种组合方式，则 $Q = $  

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
const ll inf = 2e18;
const int mod = 1e9 + 7;
ll suf[N],a[N];

ll qmi(ll a,ll b)
{
	ll res = 1;
	while(b)
	{
		if(b&1) res = res * a % mod;
		a = a * a % mod;
		b >>= 1;
	}
	return res;
}

ll inv(ll x) { return qmi(x,mod - 2) % mod; }

void solve()
{
    int n;cin >> n;
    ll p = 0,q = 0;
    q = n*(n-1)/2;
    suf[n+1] = 0;
    for(int i = 1;i <= n;i ++) cin >> a[i];
    for(int i = n;i >= 1;i --) suf[i] = suf[i+1] + a[i];
    for(int i = 1;i < n;i ++) p = (p % mod + a[i] * suf[i+1] % mod) % mod;
    cout << p * inv(q) % mod << '\n';
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}
```