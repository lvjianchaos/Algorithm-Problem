$time\ limit\ per\ test:\ 1\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$ 
$output:\ standard\ output$

**题目**

朵拉有一个包含整数的集合 $s$ 。一开始，她会将 $[l, r]$ 中的所有整数放入集合 $s$ 中。也就是说，如果且仅如果 $l \leq x \leq r$ ，整数 $x$ 最初包含在集合中。然后，她允许你执行以下操作：

- 从集合 $s$ 中选择三个**不同的**整数 $a$ 、 $b$ 和 $c$ ，使得 $\gcd(a, b) = \gcd(b, c) = \gcd(a, c) = 1^\dagger$ 。
- 然后，从集合 $s$ 中删除这三个整数。

最多可以进行多少次运算？

$^\dagger$ 回顾:  $\gcd(x, y)$ 表示整数 $x$ 和 $y$ 的 [最大公约数](https://en.wikipedia.org/wiki/Greatest_common_divisor)。

**输入**

每个测试由多个测试用例组成。第一行包含一个整数 $t$ ( $1 \leq t \leq 500$ ) - 测试用例数。测试用例说明如下。

每个测试用例的唯一一行包含两个整数 $l$ 和 $r$ ( $1 \leq l \leq r \leq 1000$ ) - 初始集合的整数范围。

**输出**

对于每个测试用例，输出一个整数 —— 您能执行的最大操作数。

**题解**

【数学】【贪心】【思维】

我们首先会发现这样的一个规律或者事实：相邻的奇数的 $gcd$ 恒等于 $1$ ；而偶数总是会有 $2$ 作为其中的因数，故 $gcd\ \ge 2$ 。
因此，贪心的去想一定是：“$2个相邻奇数+1个偶数$” 的组合可使运算次数**最大化**。
进而转为求解此区间的奇数个数。
答案为此整除于 $2$ 。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 1e8 + 9;

void solve()
{
    int l,r;cin >> l >> r;
    if(r&1) cout << (r-l+2)/2/2 << '\n';
    else cout << (r-l+1)/2/2 << '\n';
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}
```

