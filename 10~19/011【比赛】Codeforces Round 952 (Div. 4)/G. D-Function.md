**比赛场次 ** [Codeforces Round 952 (Div. 4)](https://codeforces.com/contest/1985)

**比赛题目 **G. D-Function

<!--more-->

2 seconds / 256 megabytes

standard input / standard output

## 题目

Let D(n) represent the sum of digits of n. For how many integers n where 1el≤n<1er satisfy D(k⋅n)=k⋅D(n)? Output the answer modulo 1e9+7.

## 输入

The first line contains an integer t (1≤t≤1e4) – the number of test cases.

Each test case contains three integers l, r and k (0≤l<r≤1e9, 1≤k≤1e9).

## 输出

For each test case, output an integer, the answer, modulo 1e9+7.

## 样例

**输入**

> 6
> 0 1 4
> 0 2 7
> 1 2 1
> 1 2 3
> 582 74663 3
> 0 3 1

**输出**

> 2
> 3
> 90
> 12
> 974995667
> 999

## 解释

For the first test case, the only values of n that satisfy the condition are 1 and 2.

For the second test case, the only values of n that satisfy the condition are 1, 10, and 11.

For the third test case, all values of n between 10 inclusive and 100 exclusive satisfy the condition.

## 题解

为了满足 D(k⋅n)=k⋅D(n) 的要求， n 中的每个数位 d 必须在 n 乘以 k 之后变成 k⋅d 。换句话说， n 中的任何一位数字在乘法运算后都不能转入下一位数字。由此，我们可以推导出 n 中的每个数位都必须小于或等于 ⌊9/k⌋ 。剩下的工作就是计算 10^l (含)和 10^r (不含)范围内的所有数字。

每个低于 10^r 的数字都有 r 位或更少的数字。对于少于 r 位的数字，让我们在开头填充零，直到它变成一个 r 位的数字(例如，如果是 r=5，那么 69 就变成了 00069 )。这样，我们就可以将小于 r 位的数字与精确为 r 位的数字进行同样的运算。对于每个数字，我们都有 ⌊9/k⌋+1 个选择(包括零)，而且有 r 个数字，因此满足 10^r 以下约束的数字总数是 (⌊9/k⌋+1)^r 。

要得到范围内的数字数，只需减去所有小于 10^l 的有效数字即可。因此，**<u>答案是 (⌊9/k⌋+1)^r−(⌊9k⌋+1)^l</u>**。为了快速进行指数运算，我们可以使用**快速幂**。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
const ll p = 1e9 + 7;
ll k;
// 快速幂
ll ksm(ll a, ll b){
	ll res = 1;
	while(b)
	{
		if(b & 1)res = res * a % p;
		a = a * a % p, b >>= 1;
	}
	return res;
}

void solve()
{
	ll l, r;cin >> l >> r >> k;
	if(k > 9) cout << 0 << '\n';
	else cout << ((ksm((9 / k) + 1, r) - ksm((9 / k) + 1, l)) % p + p) % p << '\n';
}

int main() {
	ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
    int _;cin >> _;
    while(_ --) solve();    
    
    return 0;
}
```

