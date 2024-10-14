
**题目：**

小苯有一个正整数 𝑥 ($0≤x≤10^{20}$)，他希望用最多一次交换数位操作（即选择 𝑥 中的两个数位交换位置）把 𝑥 变大，请问他能否做到呢。

**题解：**

【字符串】【思维】
对于任意数位，都要大于等于后一个数位，这样才不能做到；否则即可以。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
void solve()
{
    string s;cin >> s;
    int ok = 1;
    for(int i = 1;i < s.length();i ++)
    {
        if(s[i] <= s[i-1]) continue;
        else { ok = 0; break; }
    }
    if(!ok) cout << "YES\n";
    else cout << "NO\n";
}
int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}
```