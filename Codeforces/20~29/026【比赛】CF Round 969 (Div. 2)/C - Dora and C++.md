$time\ limit\ per\ test:\ 2\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$ 
$output:\ standard\ output$

**题意**

给你 $n$ 个元素组成的数组 $c$ ，并给以 $2$ 个常数 $a$ , $b$ （a , b 可能相同）。你可以进行下列操作：选择数组中任意一个元素 $c_i$ ，使其自增 $a$ 或 $b$ 。
你可以进行任意此操作。
你要使数组 $c$ 中的 $max - min$ 最小，输出该最小值。

**输入**

每个测试由多个测试用例组成。第一行包含一个整数 $t$ ( $1 \leq t \leq 10^4$ ) - 测试用例数。测试用例说明如下。

每个测试用例的第一行包含三个整数 $n$ 、 $a$ 和 $b$ ( $1 \leq n \leq 10^5$ 、 $1 \leq a, b \leq 10^9$ )--分别是数组 $c$ 的长度和常量值。

每个测试用例的第二行包含 $n$ 个整数 $c_1, c_2, \ldots, c_n$ ( $1 \leq c_i \leq 10^9$ ) - 数组 $c$ 的初始元素。

保证所有测试用例中 $n$ 的总和不超过 $10^5$ 。

**输出**

对于每个测试用例，输出一个整数 - 数组经过任意次数操作后的最小可能范围。

**题解**
###### 【数论】【裵蜀定理】
一个相对的观念，我们给一个数加上a和我们给其他数减去a，最后所得的最大值-最小值的差值一样。
显而易见，我们对每一个元素 $c_i$ 可以加上 $ax + by$ 。很容易让人想到**裵蜀定理**：$ax + by = gcd(a,b)$ ，则更新过后的 $d$ 数组：$d_i = c_i + gcd(a,b) * k_i$ (k是整数)，然后再来计算 $max - min$ 。
我们可以对 $d_i\ mod\ gcd$ ，也就是对 $c_i\ mod\ gcd$ ，这样就把最大值和最小值限制在了 $0 --\ gcd$ 之间。
对其进行升序排序。
那这样的最大-最小就对了吗？观察样例，不然。我们可以对最小值+gcd 再 - 第二小的值，不断更新最小值。 

```c++
# include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 2e5 + 9;
vector<ll> c;
void solve()
{
	c.clear();
	ll n,a,b;cin >> n >> a >> b;
	int gcd = __gcd(a,b);
	for(int i = 0;i < n;i ++) { int x;cin >> x;c.push_back(x%gcd); }
	sort(c.begin(),c.end());
	ll minn = c[n-1] - c[0];
	for(int i = 0;i < n - 1;i ++)
		minn = min(minn,c[i] + gcd - c[i+1]);
	cout << minn << '\n';
	return;
}
int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}
```

