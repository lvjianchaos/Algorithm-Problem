
$time\ limit\ per\ test:\ 2\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$
$output:\ standard\ output$

##### **Description**

Today, Sakurako was studying arrays. An array $a$ of length $n$ is considered good if and only if:

-   the array $a$ is increasing, meaning $a_{i - 1} < a_i$ for all $2 \le i \le n$;
-   the differences between adjacent elements are increasing, meaning $a_i - a_{i-1} < a_{i+1} - a_i$ for all $2 \le i < n$.

Sakurako has come up with boundaries $l$ and $r$ and wants to construct a good array of maximum length, where $l \le a_i \le r$ for all $a_i$.

Help Sakurako find the maximum length of a good array for the given $l$ and $r$.

##### **Input**

The first line contains a single integer $t$ ($1\le t\le 10^4$)  — the number of test cases.

The only line of each test case contains two integers $l$ and $r$ ($1\le l\le r\le 10^9$).

##### **Solution**

【暴力】
$while$ 循环，贪心每次差值从 1 开始 按照等差 1 增加，知道最后越界。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

void solve()
{
    ll l,r;cin >> l >> r;
    int dif = 0,cnt = 0;
    while(l <= r) { l += dif; dif ++; cnt ++; }
    cout << cnt - 1 << '\n';
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}
```