
[A. Bus to Pénjamo](https://codeforces.com/contest/2022/problem/A)

### 题意

有 $n$ 行 $2$ 列的数组表示座位，给出一组数 $a[i]$ 表示 家族 $i$ 的人数，我们定义一个好座位为：
- 同一个家族的坐在相邻座位
- 一个人坐在座位，邻座为空座
问：做了好座位的人数最多有几人？注意：所有人都要落座。

### 题解

贪心的思考，由于座位有限且所有人要落座：我们一定要尽可能的提高座位的使用率。也就是说：尽可能的让同一家族的人坐在邻座，如若还剩一人我们尽可能的让他做单个座位

##### 代码
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 2e5 + 9;
int a[110];
void solve()
{
	int n,r,cnt = 0,cnt_alone = 0;cin >> n >> r;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	for(int i = 1;i <= n;i ++) 
	{
		cnt += a[i] / 2;
		cnt_alone += a[i] % 2;
	}
	if(cnt < r) cout << min(cnt * 2 + cnt_alone,cnt * 2 + ((r - cnt) * 2 - cnt_alone) / 2 * 2 + cnt_alone % 2) << '\n';
	else cout << r * 2 << '\n';
}

int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();
	return 0;
}
```