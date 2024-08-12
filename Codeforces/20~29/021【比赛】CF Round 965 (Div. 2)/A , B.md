## A

#### 构造

给你一个坐标 ( x,y ) 和k。要你构造出k组坐标，使得 ( x[1] + x[2] + ... + x[k] ) / k = x，( y[1] + y[2] + ... + y[k] ) / k = y。

分奇偶，若为奇数，首先输出x，y；然后就像偶数一样进行操作，选取关于 (x,y) 中心对称的两点输出，直到输出够了 k 。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;

void solve()
{
	int x,y,k;cin >> x >> y >> k;
	
	if(k&1) 
		cout << x << ' ' << y << '\n',k --;
	for(int i = x + 1;i <= 1e9;i ++)
	{
		for(int j = y + 1;j <= 1e9;j ++)
		{
			if(k <= 0) return;
			int xx = 2 * x - i;
			int yy = 2 * y - j;
			cout << i << ' ' << j << '\n';
			cout << xx << ' ' << yy << '\n';
			k -= 2;
		}
	}
}

int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();	
	return 0;
}
```

## B

#### 构造

给你一组排列 p，请构造出另一组排列 q，使得这样的（ i , j ）, p[i] + p[i + 1] + ... + p[j] = q[i] + q[i + 1] + ... + q[j] ，最少。

最少是这样的（i，j）只有 1 组（1，n）。这样的 q 有多种，
如： a. 对排列p每个元素进行++/--，最大/小元素变为1/n；
	b. 对排列p进行旋转，即q = (p[2] , p[3], ... , p[n] , p[1] )。

我们发现只要使区间和不同即可。像a.的操作、b.的操作都使其不相同。

a.
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
int a[N];

void solve()
{
	int n;cin >> n;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	for(int i = 1;i <= n;i ++)
	{
		int x = a[i] + 1;
		if(x == n + 1) x = 1;
		cout << x << ' ';
	}
	cout << '\n';
}

int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();	
	return 0;
}
```
b.
```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --)
    {
        int n;cin >> n;
        vector<int> p(n);
        for(int& i: p) cin >> i;
        rotate(p.begin(), p.begin() + 1, p.end());
        for(int i: p) cout << i << " ";
        cout << "\n";
    }
}
```
##### 注: rotate()函数


> template< class ForwardIt >
> ForwardIt rotate( ForwardIt first, ForwardIt middle, ForwardIt last );
> 1） 对一系列元素执行左旋转。
> template< class ExecutionPolicy, class ForwardIt >
> ForwardIt rotate( ExecutionPolicy&& policy,  ForwardIt first, ForwardIt middle, ForwardIt last );
> 2） 与（1）相同，但根据`ExecutionPolicy&& policy`执行。

```c++
    // insertion sort
    for (auto i = v.begin(); i != v.end(); ++i)
        std::rotate(std::upper_bound(v.begin(), i, *i), i, i + 1);
    print("after sort:\t\t", v);
    
    // simple rotation to the left
    std::rotate(v.begin(), v.begin() + 1, v.end());
    print("simple rotate left:\t", v);
 
    // simple rotation to the right
    std::rotate(v.rbegin(), v.rbegin() + 1, v.rend());
    print("simple rotate right:\t", v);
```
                  
另外：

| 相关函数                                                                                                                     | 功能         |
| ------------------------------------------------------------------------------------------------------------------------ | ---------- |
| [rotate_copy](https://en.cppreference.com/w/cpp/algorithm/rotate_copy "cpp/algorithm/rotate copy")                       | 复制和旋转一系列元素 |
| [ranges::rotate ranges：：rotate](https://en.cppreference.com/w/cpp/algorithm/ranges/rotate "cpp/algorithm/ranges/rotate") | 旋转范围内元素的顺序 |
