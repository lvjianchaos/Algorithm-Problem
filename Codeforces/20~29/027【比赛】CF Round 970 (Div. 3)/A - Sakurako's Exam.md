
$time\ limit\ per\ test:\ 1\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$
$output:\ standard\ output$

##### **Description**

Today, Sakurako has a math exam. The teacher gave the array, consisting of $a$ ones and $b$ twos.

In an array, Sakurako **must** place either a '+' or a '\-' in front of each element so that the sum of all elements in the array equals $0$.

Sakurako is not sure if it is possible to solve this problem, so determine whether there is a way to assign signs such that the sum of all elements in the array equals $0$.

##### **Input**

The first line contains a single integer $t$ ($1\le t\le 100$)  — the number of test cases.

The only line of each test case contains two integers $a$ and $b$ ($0\le a,b < 10$)  — the number of '1's and the number of '2's in the array.

##### **Output**

For each test case, output "Yes" if you can make the sum of the entire array equal to $0$, and "No" otherwise.

You can output each letter in any case (lowercase or uppercase). For example, the strings "yEs", "yes", "Yes", and "YES" will be accepted as a positive answer.

##### **Solution**

【贪心】【暴力】
已知：2 = 1 + 1；我们以这样的一种贪心策略：
先将 2 两两抵消，若剩余则必剩 1 个 2；我们再用 2 个 1 来进行抵消这个 2；之后 11 互相抵消。
分类讨论得：
- 2 的个数为偶，
	- 1 的个数为偶 =》YES
	- 1 的个数为奇 =》NO
- 2 的个数为奇，
	- 1 的个数为偶$(\ge 2)$ =》YES
	- 1 的个数为奇 或 0  =》NO

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

void solve()
{
    ll a,b;cin >> a >> b;
    if(b&1)
    {
    	if(a&1 || !a) cout << "NO\n";
    	else cout << "YES\n";
    }
    else 
    {
    	if(a&1) cout << "NO\n";
    	else cout << "YES\n";
    }
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