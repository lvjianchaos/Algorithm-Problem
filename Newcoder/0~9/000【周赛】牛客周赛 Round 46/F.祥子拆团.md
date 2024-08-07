**比赛场次** 牛客周赛 Round 46

**比赛题目 **[祥子拆团](https://ac.nowcoder.com/acm/contest/84444/F)

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

Sakiko有两个数字 x,y ，她想知道，有多少种方式可以将 x 拆成 y 个正整数的乘积。

 例如 x=6,y=2 时，有 6×1=6,3×2=6,2×3=6,1×6=6这 4 种方法。

 由于这个答案可能很大，因此你需要输出答案对 1e9+7 取模后的结果。

## 输入

第一行输入一个正整数 T(1≤T≤1e3)，表示询问次数。

接下来 T 行，每行输入两个正整数 x,y(1≤x,y≤1e9)，表示询问。

## 输出

对于每个询问，在一行中输出一个整数表示答案。由于这个答案可能很大，因此你需要输出答案对 1e9+7 取模后的结果。

## 样例

**输入**

> 2
> 6 2
> 12 2

**输出**

> 4
> 6

## 解释

第 2 个询问：
[12,1],[6,2],[4,3],[3,4],[2,6],[1,12]
答案为6

## 题解

##### 【**组合计数**】【插板法】【质因子分解定理】【费马小定理】



## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll p = 1e9 + 7;

ll ksm(ll a,ll b)
{
    ll res = 1;
    while(b)
    {
        if(b&1) res = res * a % p;
        a = a * a % p;
        b >>= 1;
    }
    return res;
}

void solve()
{
    ll ans = 1;
    ll x,y;cin >> x >> y;
    for(int i = 2;i * i <= x;i ++)
    {
        if(x % i != 0) continue;
        int d = 0; // 次数
        while(x % i == 0) x /= i,d ++;
        ll temp = 1;
        for(ll j = y;j <= d + y - 1;j ++)
        {
            temp = temp * j % p;
            temp = temp * ksm(j - y + 1,p - 2) % p; // 乘法逆元
        }
        ans = ans * temp % p;
    }
    if(x > 1)
    {
        int d = 1,temp = 1;
        for(ll j = y;j <= d + y - 1;j ++)
        {
            temp = temp * j % p;
            temp = temp * ksm(j - y + 1,p - 2) % p;
        }
        ans = ans * temp % p;
    }
    cout << ans << '\n';
}
int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int _;cin >> _;
    while(_ --)
    {
        solve();
    }
    return 0;
}
```

