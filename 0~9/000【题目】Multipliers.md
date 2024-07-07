**[Cf Round 338 (Div. 2)](https://codeforces.com/contest/615)**

**[D. Multipliers][https://codeforces.com/problemset/problem/615/D]**

<!--more-->

2 seconds / 256 megabytes

standard input / standard output

## 题目

Ayrat has number *n*, represented as it's prime factorization *pi* of size *m*, i.e. *n* = *p*1·*p*2·...·*pm*. Ayrat got secret information that that the product of all divisors of *n* taken modulo 1e9 + 7 is the password to the secret data base. Now he wants to calculate this value.

## 输入

The first line of the input contains a single integer *m* (1 ≤ *m* ≤ 200 000) — the number of primes in factorization of *n*.

The second line contains *m* primes numbers *pi* (2 ≤ *pi* ≤ 200 000).

## 输出

Print one integer — the product of all divisors of *n* modulo 1e9 + 7.

## 样例

**输入**

> 2
> 2 3

**输出**

> 36

**输入**

> 3
> 2 3 2

**输出**

> 1728

## 解释

In the first sample *n* = 2·3 = 6. The divisors of 6 are 1, 2, 3 and 6, their product is equal to 1·2·3·6 = 36.

In the second sample 2·3·2 = 12. The divisors of 12 are 1, 2, 3, 4, 6 and 12. 1·2·3·4·6·12 = 1728.

## 题解

**题意**

将一个数n用素数因式p[i]表示（p可能相等），大小为m.要你求出n的所有因子的乘积取模`1e9+7`。即：所有质数p相乘等于的n的所有因子的乘积取模`1e9+7`。

**思路**

由于限时`2s`、数据范围较大，所以暴力破解一定会`TLE`。

那么我们如何去解这道题呢？

题目为什么将所有的质因子交予我们呢？没错，我们先在知道这个数n的质因子及其个数。

这时，就需要具备**质因数分解定理**的知识了。

#### 何为**质因数分解定理**？

**质因数分解定理**，也称为**唯一分解定理**，是数论中的一个基本定理，它说明了每一个大于1的自然数都可以**唯一地**分解成几个质数的乘积。这个定理的正式表述如下：

任何一个大于1的自然数N都可以**唯一地**表示为几个质数乘积的形式，即：
$$
n = p_1^{e_1} * p_2^{e_2}*···p_k^{e_k}
$$
其中，`p1,p2,···,pk`是质数，而`e1,e2,···,ek`是正整数，这个表示称为N的质因数分解。
$$
因数个数=(e_1+1)×(e_2+1)×(e_3+1)×...×(e_k+1)
$$
在这个基础上，我们或可知道这个数的**因数个数**如何求；即：通过分解这个数为质因数的乘积来计算。公式如上：这是**因为**每个质因数可以选择**从0到其指数的任意次数**来相乘，形成一个因数。因此，对于每个质因数的每个指数，都有指数加1种选择。

例如，如果一个数n的质因数分解为 `n=2^2×3^1×5^1`，那么它的因数个数就是：`(2+1)×(1+1)×(1+1)=3×2×2=12(2+1)×(1+1)×(1+1)=3×2×2=12`

所以，这个数有12个因数。

**因数个数定理**可以利用**数学归纳法**证明，本蒟蒻并不太会严格的数学证明，如有需要，还请自行查阅相关资料。

那么我们可以分析**每个素因子p对于答案的贡献**：

只含有p的因子有p,p^2,^p^3,……,p^e，而不含p的因子个数为`sum/(e + 1)`(我们不妨设总因数个数为sum)。每一个含有p的因子都会和不含p的因子组合，则每一个组合出现的次数为`sum/(e + 1)`，那么这个p对答案的贡献就是:
$$
p^{sum/(e + 1)}*p^{2*[sum/(e + 1)]}*·····*p^{k*[sum/(e + 1)]}=p^{[sum/(e + 1)]*(1+2+···+e)}=p^{[e*(1+e)/2]*[sum/(e + 1)]}=p^{[(e*sum)/2]}
$$
(另外，我们可能会想到`/2`会出现问题，但我们发现，如果e[1-k]中全为偶数，那么e可被2整除；若e[1~k]中出现一个奇数，那么那个e+1可被2整除，它组成了sum。所以如果是此时关于`/2`就不会产生问题。)

这样，loop求出每个质数的贡献相乘并记得过程中取模即可？真的是这样吗？

记得我们的sum是啥吗？
$$
sum=(e_1+1)×(e_2+1)×(e_3+1)×...×(e_k+1)
$$
**它太大了！**

所以问题来了，我们如何处理“**指数爆炸**”呢，或者说怎么降低指数呢？

#### 费马小定理

定理的内容是：

如果*p*是一个素数，那么对于任何整数*a*，都有
$$
a^{p-1}≡1(modp)
$$
换句话说，当*p*是一个素数时，*a*的*p−1*次幂除以*p*的余数总是1。

证明本蒟蒻就不自讨苦吃了，还请各位佬另寻它路orz。

这个定理可以被用来简化指数运算。例如，假设我们想要计算a^n模p的值，其中n是一个非常大的数，而p是一个素数。如果n不是p−1的倍数，直接计算a^n可能会非常困难。但是，如果我们能够将n表示为k*(p−1)+m，其中m<p−1，那么我们可以利用费马小定理来简化计算：
$$
a^{p-1}≡a^{k*(p-1)+m}≡(a^{p-1})^k*a^m(mod p)≡a^m(mod p)
$$
所以在这里，我们可以将p的指数`[e*sum)/2] MOD (p-1)`。

那我们就进而考虑`[e*sum)/2] MOD (p-1)`该如何计算。

这个/2很**影响我们边取余边除**，分类讨论：

只会有两种情况：奇/偶

1）e[i~k]只要存在一个**奇**数，那么在求sum的过程中将第一个遇到的奇数/2再取余，其他照样即可；

2）若全为**偶数**，那么我们sum照算，只要将每个e/2即可。

剩下的就交给**快速幂**(若不知道`ksm`可看代码注释~~板子如下~~)了。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int M = 2e5 + 9;
const int MOD = 1e9 + 7;
ll p[M],c_p[M]; // 分别表示质因子，质因子个数
vector<ll> v; // 存储质因子，无重复，方便后续操作
// 快读
inline int read(){
    int x = 0, f = 1; char ch = getchar();
    while(ch < '0' || ch > '9'){ if(ch == '-') f = -1; ch = getchar(); }
    while(ch >= '0' && ch <= '9'){ x = x * 10 + (ch ^ 48); ch = getchar(); }
    return x * f;
}
// 快速幂=>倍增思想
ll ksm(ll base,ll power)
{
    // base => 底
    // power => 幂
    ll res = 1;
    while(power)
    {
        if(power & 1) res = res * base % MOD; // power为奇数，
        base = base * base % MOD;
        power >>= 1; // 必然会有power变为1的时刻，此时base即为所求
        // power有两种情况：偶数时，底数平方，power整除2；
        // 				  奇数时，底数平方，幂整除2向下取整，还有1项在此过程中被搞没了，所以在此之前还要在原基础上乘以底数
    }
    return res;
}
int main(void)
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0); // 关闭同步流
    // 初始化与输入
	int m = read();
	ll sum = 1; // 因子总数
    ll ans = 1; // 答案
    bool fg = 1; // 用于判断质因子数中是否存在奇数
	for(int i = 1;i <= m;++ i)
	{
		p[i] = read();
		if(!c_p[p[i]]) v.push_back(p[i]); // 如果未出现过，push入vector中
		c_p[p[i]] ++; // 个数++
	} 
	
	for(auto &i :v) if(c_p[i] & 1) fg = 0; // 判断出现过奇数
    
	if(fg) // 如果不存在奇数，即c_p[i]都是偶数
	{
		for(auto &i :v)
        {
            sum = sum * (c_p[i] + 1) % (MOD - 1); // 求因子总数，注意一定要在修改c_p[i]前
            c_p[i] >>= 1;// 相当于整除2
        }
		for(auto &i :v) ans = ans * ksm(i,sum * c_p[i] % (MOD - 1)) % MOD;
	}
	else // 存在奇数
	{
		sum = 1;
		bool wc = 1; // 只是随便找一个奇数在算sum时整除2
		for(auto &i :v)
		{
			if(c_p[i]&1 && wc)
			{
				wc = 0;
				sum = sum * (c_p[i] + 1) / 2 % (MOD - 1);
			}
			else sum = sum * (c_p[i] + 1) % (MOD - 1);
		}
		for(auto &i :v) ans = ans * ksm(i,sum * c_p[i] % (MOD - 1)) % MOD;
	}
	cout << ans << '\n';
	return 0;
}
```