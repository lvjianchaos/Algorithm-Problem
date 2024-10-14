
**题目**：

定义 k- 好数为：可以表示为若干个不同的 𝑘 的整数次幂之和的数字。

例如：$30=3^3+3^1$  ，因此 $30$ 是一个 $\texttt{3-}$ 好数，而 2 不是一个 $\texttt{3-}$ 好数（虽然有：$2=3^0+3^0$ ，但好数要求次幂数字不同）。  
小苯有一个整数 n，他想知道 n 最少可以被表示成几个 $\texttt{k-}$ 好数的和，请你帮帮他吧。
$(1≤n≤10^{18}) , (1≤k≤10^{18})$

**题解**：

【数学】

本题一定有解（理论上讲，本题应该加上一个无解输出 $NO$ 的要求的，需要选手证明这一点）；

观察得到：
- k > n , 需要表示成 n 个 $k^0$ ，故答案为 n ；
- k = n，只需要表示成 1 个 $k^1$ ，故答案为 1；
- k = 1，则可以表示成 1 个 $k^1 + k^2 + …… + k^{n}$ ，故答案为 1；（赛时就是这个未特判导致超时未过）
仔细观察题目：k- 好数，由k的幂次方相加组成。本质上是 k 进制下的数且各数位大小不超过1。
n 即是这样的 数 相加得到。
则即可把 n 化为 k 进制数，答案就为 数位最大的那个数。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
void solve()
{
    ll n,k;cin >> n >> k;
    if(k == 1) cout << 1 << '\n';
    else
    {
        ll ans = 0;
        ll a = n,b = 0;
        while(a)
        {
            b = a % k;a /= k;
            ans = max(ans,b);
        }
        cout << ans << '\n';
    }
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}
```
