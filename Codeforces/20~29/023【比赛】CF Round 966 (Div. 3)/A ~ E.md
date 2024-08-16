## A - Primary Task

有一个整数 $n$ 以 $10^x$ ($x \ge 2$)为格式，现在 `^` 失去表达，你得到这个不表达的整数 $a$  。例如：$10^5$ $\rightarrow$  $105$ ，$10^{19}$ $\rightarrow$  $1019$ 。让你判断给出的 $a$ 是否可以还原为 $n$ 的格式。

判断前两位是否为 $10$ ，再判断后面是否大于 $1$ 。
###### 【基础语法】【string】

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll inf = 0x3f3f3f3f;
const ll p = 1e9 + 7;
const ll N = 2e5 + 9;

void solve()
{
	string x;cin >> x;
	if(x.size() <= 2)
	{
		cout << "NO\n";
	}
	else if(x.size() == 3)
	{
		if(x[0] == '1' && x[1] == '0')
		{
			if(x[2] > '1') cout << "YES\n";
			else cout << "NO\n";
		}
		else cout << "NO\n";
	}
	else
	{
		if(x[0] == '1' && x[1] == '0')
		{
			if(x[2] > '0') cout << "YES\n";
			else cout << "NO\n";
		}
		else cout << "NO\n";
	}
}

int main(void)
{
	int _ = 1; cin >> _;
	while(_ --) solve();
	return 0;
}
```

## B - Seating in a Bus

给你一个长度为 $n$ 的数组 $a$ ，下标先后表示按时间先后乘上公交的人，$a[i]$ 表示座位号，要求乘客按照下面的规定乘坐：
	1. 公交无人，任意座位可做；
	2. 公交有人，只有邻边有人才可做；
给你这样的数组，判断是否按规定乘坐。

我们只需要用一个桶记录以下座位是否有人，在判断的时候，若公交上已有人了，判断这位上车的乘客的前或后是否有人即可。
###### 【记忆】【桶】
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll inf = 0x3f3f3f3f;
const ll p = 1e9 + 7;
const ll N = 2e5 + 9;
ll a[N],c[N];

void solve()
{
	memset(c,0,sizeof(c));    // initialize the array c which record the appearance of a[i]
	// input
	int n;cin >> n;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	// solve
	for(int i = 1;i <= n;i ++)
	{
		// if there have been pepole on the bus
		if(i > 1)
		{
			// if the last or next seat has been occupied
			if(c[a[i] - 1] || c[a[i] + 1]) 
			{
				c[a[i]] ++;
			}
			// if not, prove there're someone breaking the rules
			else
			{
				cout << "NO\n";
				return;
			}
		}
		// if no pepole
		else c[a[i]] ++;
	}
	cout << "YES\n";
}

int main(void)
{
	int _ = 1; cin >> _;
	// multicase
	while(_ --) solve();
	return 0;
}
```

## C - Numeric String Template

有一个长度为 $n$ 的数组 $a$ ，称作模板。还有 $m$ 个字符串，编号从 $1$ 到 $m$ 。问是否该字符串与模板匹配。
匹配的意思是：
 - 字符串 $s$ 的长度等于数组 $a$ 中的元素数 
 - $a$ 的相同数字对应于 $s$ 的相同符号。因此，如果 $a_i = a_j$，则 $s_i = s_j$ 为 （$1 \le i， j \le n$） 
 - $s$ 的相同符号对应于 $a$ 的相同数字。因此，如果 $s_i = s_j$，则 $a_i = a_j$ 为 （$1 \le i， j \le n$）
	*换句话说，字符串的字符和数组的元素之间必须存在一一对应关系*
 例如，如果 $a$ = \[$3， 5， 2， 1， 3$\]，则字符串“abfda”与模板匹配，而字符串“afbfa”则不匹配，因为字符“f”对应于数字 $1$ 和 $5$。

用两个`map`构建一一对应关系。利用`count()`判断是否建立过了联系。
###### 【赛后hack】
本以用`c[]`桶存储字母对应整数，判断是否存在时，用了`!=0`，但是整数的范围是 $[-1e9,1e9]$ 。做题时注意范围啊！！！/(ㄒoㄒ)/
###### 【map】
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;

const ll inf = 0x3f3f3f3f;
const ll p = 1e9 + 7;
const ll N = 2e5 + 9;

ll a[N];
map<int,char> mp1;
map<char,int> mp2;

// notice: the integer a[i] range from -1e9 to 1e9;

void solve()
{
	// input
	int n;cin >> n;
	for(int i = 0;i < n;i ++) cin >> a[i];
	int m;cin >> m;
	// solve
	while(m --)
	{
		string s;cin >> s;
		bool fg = 1;	// if it is ok
		// initialize
		mp1.clear();
		mp2.clear();
		// condition 1
		if(s.length() == (ull)n)
			for(int i = 0;i < n;i ++)
			{
				// condition 2
				if(!mp1.count(a[i])) mp1[a[i]] = s[i];
				else if(fg && mp1[a[i]] != s[i]) { fg = 0; break; } // if not satisfied, fg = 0
				// condition 3
				if(!mp2.count(s[i])) mp2[s[i]] = a[i];
				else if(fg && mp2[s[i]] != a[i]) { fg = 0; break; } // if not satisfied, fg = 0
			}
		else fg = 0; // if not satisfied condition1, fg = 0
		// if satisfied all conditions, output "YES";otherwise,"NO".
		if(fg) cout << "YES\n";
		else cout << "NO\n";
	}
}

int main(void)
{
	// multicase
	int _ = 1; cin >> _;
	while(_ --) solve();
	return 0;
}
```

## D - Right Left Wrong

Vlad found a strip of $n$ cells, numbered from left to right from $1$ to $n$. In the $i$\-th cell, there is a positive integer $a_i$ and a letter $s_i$, where all $s_i$ are either 'L' or 'R'. 
Vlad invites you to try to score the maximum possible points by performing any (possibly zero) number of operations. 
In one operation, you can choose two indices $l$ and $r$ ($1 \le l < r \le n$) such that $s_l$ = 'L' and $s_r$ = 'R' and do the following: 
- add $a_l + a_{l + 1} + \dots + a_{r - 1} + a_r$ points to the current score; 
- replace $s_i$ with '.' for all $l \le i \le r$, meaning you can no longer choose these indices.
For example, consider the following strip: 
| $3$ | $5$ | $1$ | $4$ | $3$ | $2$ | 
| L | R | L | L | L | R | 
You can first choose $l = 1$, $r = 2$ and add $3 + 5 = 8$ to your score. 
| $3$ | $5$ | $1$ | $4$ | $3$ | $2$ | 
| .  | .  | L | L | L | R | 
Then choose $l = 3$, $r = 6$ and add $1 + 4 + 3 + 2 = 10$ to your score. 
| $3$ | $5$ | $1$ | $4$ | $3$ | $2$ | 
| .  | .  | .  | .  | .  | .  | 
As a result, it is impossible to perform another operation, and the final score is $18$. 
What is the maximum score that can be achieved?

贪心的思考如何最大化得分？因为使用过 ’L‘ 和 ’R‘ 后，中间的可能的 'L' 和 ’R‘ 的组合会被消灭。为了避免这样的坏情况，我们从中间开始寻找 ’L‘ ’R‘ 组合，这样所有的组合就可以用到，但是可能的’L‘ 'R'组合并不是最优的。所以反过来想，我们可以从首尾两边进行移动找取组合，这样就可以最大化组合的收益。
###### 【赛后hack】
指针可能在`while`循环中超出`s`的范围造成`RE`。在循环中加入限制双指针范围的条件。
###### 【双指针】【模拟】【贪心】
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll inf = 0x3f3f3f3f;
const ll p = 1e9 + 7;
const ll N = 2e5 + 9;

ll a[N];

void solve()
{
    // input
	int n;cin >> n;
	for(int i = 0;i < n;i ++) cin >> a[i];
	string s;cin >> s;
	ll ans = 0;
	
	for(int i = 0;i < n;i ++) if(i > 0) a[i] += a[i - 1]; // prefix array
	for(int i = 0,j = s.size() - 1;i < j;)
	{
		while(s[i] != 'L' && i <= j) i ++;    // find the L
		while(s[j] != 'R' && i <= j) j --;    // find the R
		if(i >= j) break;           // if finished, break
		ans += a[j] - (i == 0 ? 0 : a[i - 1]); // add the score to the ans
		i ++,j --; // continue moving
	}
	// output
	cout << ans << '\n';
}

int main(void)
{
	int _ = 1; cin >> _;
	while(_ --) solve();
	return 0;
}
```

## E - Photoshoot for Gorillas

You really love gorillas, so you decided to organize a photoshoot for them. Gorillas live in the jungle. The jungle is represented as a grid of $n$ rows and $m$ columns. $w$ gorillas agreed to participate in the photoshoot, and the gorilla with index $i$ ($1 \le i \le w$) has a height of $a_i$. You want to place **all** the gorillas in the cells of the grid such that there is **no more than one gorilla** in each cell.
The spectacle of the arrangement is equal to the sum of the spectacles of all sub-squares of the grid with a side length of $k$. The spectacle of a sub-square is equal to the sum of the heights of the gorillas in it. 
From all suitable arrangements, choose the arrangement with the **maximum** spectacle.

为了最大化奇观，我们贪心的思考：大对大。每个格子的贡献是不同且可计算的，我们一定是让大贡献的格子放上高的猩猩。观察发现，贡献呈四角对称。求解贡献并不难，将横纵方向分离后，本质上可以直接通过区间的交得到，这里不再展开，具体看代码。
###### 【贪心】【数学】
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll inf = 0x3f3f3f3f;
const ll p = 1e9 + 7;
const ll N = 2e5 + 9;

ll a[N];         // the height of each gorilla
vector<ll> b;    // the contribution of every cell

void solve()
{
	// input
	int n,m,k;cin >> n >> m >> k;
	int w;cin >> w;
	for(int i = 1;i <= w;i ++) cin >> a[i];
	// sort
	sort(a + 1,a + 1 + w,greater<ll> ());
	// clear
	b.clear();
	// store every cell's contribution
	for(ll i = 1;i <= (n+1)/2;i ++)
	{
		for(ll j = 1;j <= (m+1)/2;j ++)
		{
			ll x = min((n + 1)/2,min(k,max(1,n-k+1)));
			ll y = min((m + 1)/2,min(k,max(1,m-k+1)));
			
			ll ii = min(x,i);
			ll jj = min(y,j);
			// cout << ii << ' ' << jj << "&&&\n";
			if((n&1 && i == n/2+1) || (m&1 && j == m/2+1))
			{
				if(n&1 && i == n/2+1 && m&1 && j == m/2+1)
				{
					b.push_back(ii*jj);
				}
				else
				{
					b.push_back(ii*jj);
					b.push_back(ii*jj);
				}
			}
			else
			{
				b.push_back(ii*jj);
				b.push_back(ii*jj);
				b.push_back(ii*jj);
				b.push_back(ii*jj);
			}
			
		}
	} 
	// sort
	sort(b.begin(),b.end(),greater<ll>());
	ll ans = 0;
	for(int i = 1;i <= w;i ++)
	{
		// cout << b[i - 1] << '*' << a[i] << '\n';
		ans += b[i - 1] * a[i];
	}
	cout << ans << '\n';
}

int main(void)
{
	int _ = 1; cin >> _;
	while(_ --) solve();
	return 0;
}
```