
$time\ limit\ per\ test：1\ second$
$memory\ limit\ per\ test：1024\ megabytes$
###### 题意
有两个按钮的计数器，按下 "+"按钮，计数器的数值将增加 $1$ ；按下 "c "按钮，计数器的数值将设置为 $0$ 。计数器的初始值为 $0$ 。

进行 $n$ 次操作，按下其中一个。有 $m$ 个已知的条件 — i-th 条件表示：经过 $a[i]$ 次操作后，计数器上值为 $b[i]$。

有 $T$ 组样例。 $1<=n<=10^9$； $1<=m<=10^5$； $1<=a[i]<=n$；$0<=b[i]<=10^9$ ；保证所有测试样例的 $m$ 之和不超过 $5×10^5$。

###### 【基础】【模拟】

思考：如果现在的值比前一步的要大，说明：可能1. 一直在摁下 "+" ; 2. 或者中间摁了 "c"。
	反之，说明：他一定至少摁了 $1$ 次 "c"，其余摁下 "+"。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll inf = 0x3f3f3f3f;
const ll p = 1e9 + 7;
const ll N = 2e5 + 9;

int n,m; // the numbers of operations and conditions
pair<ll,ll> pp[N]; // pair type to store the data a[i] ans b[i]

void solve()
{
	// define the answer: bool "Yes" or "No"
	bool flag = false;
	// input
	cin >> n >> m;
	for(int i = 1;i <= m;i ++)
	cin >> pp[i].first >> pp[i].second;
	// ascending sort
	sort(pp + 1,pp + 1 + m);
	// solve
	for(int i = 1;i <= m;i ++)
	{
		int dif_fst = pp[i].first - pp[i - 1].first;
		int dif_scd = pp[i].second - pp[i - 1].second;
		// 一直没过的原因，a[i]可能会重复出现(真的服了)
		if(!dif_fst)
		{
			if(!dif_scd) continue;
			else { flag = true; break; }
		}
		// divided into 2 situations:
		// if it increases, maybe only press button "+" 
		// or press "c" several   times(just justifing the minimum is okay or not)
		// if it decreases, it is a must that we press the button
		//  "c"(just justifing the minimum is okay or not)
		if(dif_scd > 0) { if(dif_scd != dif_fst && pp[i].second + 1 > dif_fst) { flag = true; break; } }
		else { if(1 + pp[i].second > dif_fst) { flag = true; break; } }
}
// output
	cout << (flag ? "No\n" : "Yes\n");
}

int main(void)
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	// multicase
	int _ = 1; cin >> _;
	while(_ --) solve();
	return 0;
}
```

官方题解：

如果第 $a_i$ 次操作后计数器的值为 $b_i$，说明第 ($a_i - b_i$) 次操作是清零，且 $[a_i - b_i +1,a_i]$ 这个操作区间内没有清零操作。

把区间排序后进行判断即可。复杂度 $O(nlogn)$ 。


```c++
#include <bits/stdc++.h> 
using namespace std;

int n; 

void solve() 
{ 
	scanf("%*d%d", &n); 
	// mp[L] 表示 L 是一个清零操作，且从 L + 1 到 mp[L] 都是加一操作 
	map<int, int> mp; 
	bool failed = false; 
	for (int i = 1; i <= n; i++) 
	{ 
		int x, y; scanf("%d%d", &x, &y); 
		// 计数器的值不能比操作数还大 if (y > x) failed = true; 
		// 记录必须是清零操作的位置，以及加一操作的区间 
		int &t = mp[x - y]; 
		t = max(t, x); 
	} 
	if (failed) 
	{ 
		printf("No\n"); return; 
	} 
	// R 表示清零操作的“禁区”的最右端 
	int R = -1; 
	for (auto &p : mp) 
	{ 
		// 需要在“禁区”里执行清零操作，无解 
		if (R >= p.first) 
		{ 
			printf("No\n"); return; 
		} 
		// 更新“禁区”的最右端 
		R = p.second; 
	} 
	printf("Yes\n"); 
} 

int main() 
{ 
	int tcase; 
	scanf("%d", &tcase); 
	while (tcase--) solve(); 
	return 0; 
}
```