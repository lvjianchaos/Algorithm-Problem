**比赛场次**	[The 2022 ICPC Asia Hangzhou Regional Programming Contest](https://codeforces.com/gym/104090)

**比赛题目**	Modulo Ruins the Legend

<!--more-->

1 seconds / 1024 megabytes

standard input / standard output

## 题目

给一组整数序列 a[n] ，你要选择非负整数 s,d，向序列中每个元素加上 s + i · d。你要让加上之后总和%m的值最小。

也就是说 (sum(a[n]) + n * s + n*(n + 1) / 2 * d) % m 值最小。

## 输入

第一行两个整数 n , m (1 <= n <= 1e5 , 1 <= m <= 1e9)；

第二行包含 n 个整数 a1, a2 , …… ，a[n] (0 <= a[i] < m)，表示初始序列。

## 输出

输出两行。

第一行一个整数，表示以 m 为模数的最小元素和；

第二行两个整数 s , d (0 <= s , d < m)，表示选择的两数，如果有多解，输出任意解即可。

## 样例

**输入**

> 6 24
> 1 1 4 5 1 4

**输出**

> 1
> 0 5

**输入**

> 7 29
> 1 9 1 9 8 1 0

**输出**

> 0
> 0 0

## 题解

[[板子#2. 扩展欧几里得求逆元]]
##### 【数论】【exgcd扩展欧几里得算法】【裵蜀定理】 
> 设 sum(a[n]) = sum，
> 	sum + s * n + n(n + 1)/2 * d (mod m)          (原式)
> 	<=> sum + s * n + n(n + 1)/2 * d + t * m    (同余定义)
> 		<=> s * n + n(n+1)/2 * d = k1 * gcd(n,n(n+1)/2)    (裴蜀定理)
> 	<=>  sum + k1 * gcd(n,n(n+1)/2) + t * m    
> 		<=>  k1 * gcd(n,n(n+1)/2) + t * m = k2 * gcd(n,m,n(n+1)/2)    (裴蜀定理)
> 	<=> sum + k2 * gcd(n,m,n(n+1)/2)
> 	这就是最小值。最小值就是sum减去足够倍数的gcd(n,m,n(n+1)/2)。等价为：sum % gcd(n,m,n(n+1)/2)
> 	记 ans = sum % gcd(n,m,n(n+1)/2)
> 那么如何解出 s，d 的值呢？
> 我们现在已知：
> 	（1）sum + k1 * gcd(n,n(n+1)/2) + t * m = ans
> 	（2）s * n + n(n+1)/2 * d = k1 * gcd(n,n(n+1)/2)
> （1）移项：k1 * gcd(n,n(n+1)/2) + t * m = ans - sum
> 根据exgcd就可解此方程，将所得解再代入（2）式再使用一次扩展欧几里得算法即可解出s , d。但这里不确定s,d是否是负数或是否小于m，所以再进行一次取模m。

## 代码

```c++
#include <bits/stdc++.h>
using ll = long long;
using namespace std;

const int N = 1e5 + 5;
ll n, m, sum, x, ans, k1, t;

// 欧几里得算法 
ll gcd(ll a,ll b) {return b == 0 ? a : gcd(b , a % b);}
// 扩展欧几里得算法: ax + by = 1 求解x,y
ll exgcd(ll a, ll b, ll &x, ll &y) 
{
    if (!b) 
    {
        x = 1;
        y = 0;
        return a;
    }
    ll d = exgcd(b, a % b, y, x);
    y -= (a / b) * x;
    return d;
}

int main () 
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	// input
    cin >> n >> m;
    for (int i = 1; i <= n; i++)   cin >> x, sum += x;
    
    ll a = n, b = n * (n + 1) / 2, g1 = __gcd (a, b), g2 = __gcd (g1, m);
    ans = sum % g2;
    // 可提取公因数 g2
    g1 /= g2, m /= g2;
    exgcd (g1, m, k1, t);
    // 由于exgcd板子是对右式=1的解，所以得到解后还要乘以右式，下同理
    (k1 *= ((ans - sum) / g2) % m) %= m;	// 由于后续无关于t的操作故不再进行处理，只对k1进行了处理。
    // 可提取公因数 g1
    a /= g1, b /= g1;
    ll s, d;
    exgcd (a, b, s, d);
    (s *= k1) %= m, (d *= k1) %= m;
    // 保证 x 和 y非负且小于m
    (s += m) %= m, (d += m) %= m;
    // output
    cout << ans << '\n';
    cout << s << ' ' << d << '\n';
    return 0;
}

```

