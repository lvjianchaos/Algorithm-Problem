
$time\ limit\ per\ test:\ 2\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$
$output:\ standard\ output$

##### **Description**

For a certain permutation $p$$^{\text{∗}}$ Sakurako calls an integer $j$ reachable from an integer $i$ if it is possible to make $i$ equal to $j$ by assigning $i=p_i$ a certain number of times.

If $p=[3,5,6,1,2,4]$, then, for example, $4$ is reachable from $1$, because: $i=1$ $\rightarrow$ $i=p_1=3$ $\rightarrow$ $i=p_3=6$ $\rightarrow$ $i=p_6=4$. Now $i=4$, so $4$ is reachable from $1$.

Each number in the permutation is colored either black or white.

Sakurako defines the function $F(i)$ as the number of black integers that are reachable from $i$.

Sakurako is interested in $F(i)$ for each $1\le i\le n$, but calculating all values becomes very difficult, so she asks you, as her good friend, to compute this.

$^{\text{∗}}$A permutation of length $n$ is an array consisting of $n$ distinct integers from $1$ to $n$ in arbitrary order. For example, $[2,3,1,5,4]$ is a permutation, but $[1,2,2]$ is not a permutation (the number $2$ appears twice in the array), and $[1,3,4]$ is also not a permutation ($n=3$, but the array contains $4$).

##### **Input**

The first line contains a single integer $t$ ($1\le t\le 10^4$)  — the number of test cases.

The first line of each test case contains a single integer $n$ ($1\le n\le 2\cdot 10^5$)  — the number of elements in the array.

The second line of each test case contains $n$ integers $p_1, p_2, \dots, p_n$ ($1\le p_i\le n$)  — the elements of the permutation.

The third line of each test case contains a string $s$ of length $n$, consisting of '0' and '1'. If $s_i=0$, then the number $p_i$ is colored black; if $s_i=1$, then the number $p_i$ is colored white.

It is guaranteed that the sum of $n$ across all test cases does not exceed $2\cdot 10^5$.

##### **Output**

For each test case, output $n$ integers $F(1), F(2), \dots, F(n)$.

##### **Solution**

【模拟】

模拟过程，发现其中的元素总是可以构成一个 **循环节** ，不管是到达自己还是其他。
而在这个循环节单元的所有元素的可到达的数是一样的，也就是说到达黑色的个数也是一样的。、知道这个后，就对其进行模拟即得答案。
虽然代码里是嵌套循环，但实际上还是 $O(n)$

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
ll c[N],a[N];

void solve()
{
    int n;cin >> n;
    for(int i = 1;i <= n;i ++)
    {
    	c[i] = 0;
    	cin >> a[i];
    }
    string s;cin >> s;s = " " + s;
    for(int i = 1;i <= n;i ++)
    {
    	if(c[i]) continue;
    	ll sta = i,cnt = 0;
    	vector<ll> tmp;
    	tmp.push_back(i);
    	if(s[i] == '0') cnt ++;
    	while(a[i] != sta)
    	{
    		i = a[i];
    		tmp.push_back(i);
    		if(s[i] == '0') cnt ++;
    	}
    	for(auto &val : tmp) c[val] = cnt; 
    	i = sta;
    }
    for(int i = 1;i <= n;i ++) cout << c[i] << ' ';
    cout << '\n';
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

##### **遗憾的是**

上述代码赛后重测 **TLE** 了。为此，我做出修改，具体思路仍然一样。其实是进行循环2次循环节，第二遍是打上个数。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
ll c[N],a[N],vis[N];

void solve()
{
    int n;cin >> n;
    for(int i = 1;i <= n;i ++)
    {
    	c[i] = 0;
    	vis[i] = 0;
    	cin >> a[i];
    }
    string s;cin >> s;s = " " + s;
    for(int i = 1;i <= n;i ++)
    {
    	if(c[i]) continue;
    	vis[i] = 1;
    	ll x = a[i],cnt = 0;
    	while(x != i)
    	{
    		if(s[x] == '0') cnt ++;
    		vis[x] = 1;
    		x = a[x];
    	}
    	c[i] = cnt;
    	x = a[i];
    	while(x != i)
    	{
	    	c[x] = cnt;
	    	x = a[x];
    	}
    }
    for(int i = 1;i <= n;i ++) cout << c[i] << ' ';
    cout << '\n';
    return;
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}