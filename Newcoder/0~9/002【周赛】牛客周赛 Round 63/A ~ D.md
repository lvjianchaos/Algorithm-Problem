## A - 小红的好数

小红定义一个正整数是“好数”，当且仅当该数满足以下两个性质：  
1. 数位恰好为2。  
2. 个位数和十位数相同。

请你判断一个给定的正整数是否是好数？

#### 题解：
按照题目模拟判断即可

###### 代码
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 2e5 + 9;

void solve()
{
	string s;cin >> s;
    if(s.size() == 2)
    {
        if(s[0] == s[1]) cout << "Yes\n";
        else cout << "No\n";
    }
    else cout << "No\n";
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t = 1; // cin >> t;
	while(t --)
	{
		solve();
	}
	return 0;
}
```

## B - 小红的好数组

#### 题意

小红定义一个数组是“好数组”，当且仅当该数组满足以下两个性质：  
1. 该数组不是回文数组。  
2. 修改恰好一个元素后，该数组变成回文数组。  
所谓回文数组，即将一个数组左右翻转后，和原数组相同，例如$[12,3,12]$是回文数组。  
  
现在小红拿到了一个数组，请你帮小红计算有多少个长度为kkk的连续子数组是好数组。

#### 题解

暴力模拟
###### 代码
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 2e5 + 9;
ll a[1111];
void solve()
{
	int k,n,ans = 0;cin >> n >> k;
    for(int i = 1;i <= n;i ++) cin >> a[i];
    for(int i = 1;i <= n - k + 1;i ++)
    {
        int cnt = 0;
        for(int ed = i + k - 1,st = i;ed > st;st ++,ed --)
            if(a[ed] == a[st]) cnt ++;
        if(cnt == k / 2 - 1) ans ++;
    }
    cout << ans << '\n';
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t = 1; // cin >> t;
	while(t --)
	{
		solve();
	}
	return 0;
}
```

## C - 小红的矩阵行走

#### 题意

现在给定了一个矩阵，小红初始站在矩阵的左上角。已知小红每次可以向右或者向下走一步，当小红经过一个格子时，她将收集该格子的正整数。小红希望到达右下角时，收集到的所有正整数都相同。你能帮帮她吗？

#### 题解

**DFS**

###### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;

ll a[111][111];
int n,m;

bool ok = 0;

void dfs(int x,int y)
{
    int b = a[x][y];
    if(ok == 1) return;
    if(x == n && y == m) {ok = 1;return;}
    if(x < n && a[x+1][y] == b) dfs(x+1,y);
    if(y < m && a[x][y+1] == b) dfs(x,y+1);
    return;
}

void solve()
{
    ok = 0;
	cin >> n >> m;
    for(int i = 1;i <= n;i ++)
        for(int j = 1;j <= m;j ++) cin >> a[i][j];
    
    dfs(1,1);
    
    if(ok) cout << "Yes\n";
    else cout << "No\n";
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --)
	{
		solve();
	}
	return 0;
}
```

## D - 小红的行列式构造

#### 题意
  
小红希望你构造一个3阶行列式，满足每个元素的绝对值不小于1，且行列式的值等于xxx。你能帮帮她吗？

#### 题解

**构造**

线性代数，行列式的性质，上下两行+-，行列式结果不变
###### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
int main()
{
    // ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int x;cin >> x;
    if(x == 0) printf("1 1 1\n1 1 1\n1 1 1");
    else printf("2 %d 1\n1 %d 1\n1 %d 2",x,x,x);
    return 0;
}
```