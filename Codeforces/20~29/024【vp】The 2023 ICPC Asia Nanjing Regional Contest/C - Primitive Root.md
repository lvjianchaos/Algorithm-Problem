
$time\ limit\ per\ test:\ 1\ second$
$memory\ limit\ per\ test:\ 1024\ megabytes$

给你一个质数 $P$ ( $2<=P<=10^{18}$ )，和一个整数 $m$ ( $1<=m<=10^{18}$ ) ; 有多少个非负整数 $g$ 满足 $g≤m$ 和  $g⊕(P−1)≡1(mod\ P)$ ? 这里的 $⊕$ 是位异或(XOR)运算符。
有多组样例 $T$ ( $1<=T<=10^5$ )

###### 【数学】【位运算】【异或】

由题意得：$g⊕(P−1)≡1(mod\ P)$
同余定理：$g⊕(P−1)≡1 + kP$    $k∈N$
异或性质：$g≡(1 + kP)⊕(P−1)$
由异或的性质：$a-b<=a⊕b<=a+b$，
得：$(1 + kP)-(P−1)<=g<=(1 + kP)+(P−1)$
即：$P(k-1)+2<=g<=P(k+1)$
又：$g <= m$
1. $P(k+1)\le m\ => k \le \lfloor m/p \rfloor - 1$，$k∈[0, \lfloor m/p \rfloor-1]$ 合法=> $m/p$个
2. $P(k-1)+2 \ge m\ => k \ge \lceil (m-2)/p \rceil+1$ ，不合法
3. 对$k∈[\lfloor m/p \rfloor,\lceil (m-2)/p \rceil]+1$，不确定，需要判断

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
	// input
	ll p,m; cin >> p >> m;
	// 最大值小于 m ,则 g 一定成立，故直接加
    ll cnt = m / p;
	// 求不确定区间的左/右端点
    ll l = m / p, r = (m - 2) / p + ((m - 2) % p != 0) + 1;
    // 判断不确定区间的情况
    for(ll k = l;k <= r; k ++)
        if(((k * p + 1) ^ (p - 1)) <= m) cnt ++;
    cout << cnt << '\n';
    return;
}

int main()
{
    ios::sync_with_stdio(0);cin.tie(0),cout.tie(0);
    int _ = 1; cin >> _;
	while(_ --) solve();
    return 0;
}
```
