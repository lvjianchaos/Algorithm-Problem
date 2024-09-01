$time\ limit\ per\ test:\ 3\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$ 
$output:\ standard\ output$

**题意：**

给你一个有根树（根为 $1$ 节点），任意顶点（$1<i \le n$ ），有一条边连接 $i$ 和 $p_i$ ( $1\le p_i < i$ )，其权重为 $t_i$ 。
我们只知道 $\displaystyle\sum_{i=2}^n t_i = w$ 和 $t_i$ 是非负整数。
树的节点编号是 $dfs序$ —— 每个子树的顶点编号都是连续的整数。
定义 $dis(u,v)$ 为该树中节点 $u$ 和 $v$ 的简单路径的长度。
接下来，会有 $n - 1$ 个 事件：
* 给你整数 $x$ , $y$ ，表示 $t_x = y$ .
在每次事件后，给出最大可能值的和 $sum\{max[dis(i,i\ mod \ n+1)]\}$ 
请注意，在计算 $i≠j$ 的最大可能值 $dist(i,i\ mod\ n+1)$ 和 $dist(j,j\ mod\ n+1)$ 时，未知边的权重**可能不同**。

**输入：**

每个测试由多个测试用例组成。第一行包含一个整数 $t$ ( $1 \leq t \leq 10^4$ )--测试用例数。( $1 \leq t \leq 10^4$ )--测试用例的数量。测试用例说明如下。

每个测试用例的第一行包含两个整数 $n$ 和 $w$ ( $2 \le n \le 2 \cdot 10^5$ , $0 \leq w \leq 10^{12}$ )--树的顶点数和边的权重之和。

每个测试用例的第二行包含 $n - 1$ 个整数 $p_2, p_3, \ldots, p_n$ ( $1 \leq p_i < i$ ) --树的边缘描述。

然后是 $n-1$ 行，表示事件。每行包含两个整数 $x$ 和 $y$ ( $2 \leq x \leq n$ ， $0 \leq y \leq w$ )，表示 $t_x = y$ 。

可以保证事件中的所有 $x$ 都是不同的。还可以保证所有 $y$ 的和等于 $w$ 。

保证所有测试用例中 $n$ 的总和不超过 $2 \cdot 10^5$ 。

**输出：**

对于每个测试用例，输出一行包含 $n-1$ 的整数，每个整数代表每个事件后的答案。

**题解：**

首先，在由 $dfs$ 序编号的树中， $i$ 和 $i+1$ 节点只会有两种关系：
- 直接相连（即 $i$ 是 $i + 1$ 的父节点）
- $lca(i,i+1) = fa[i+1]$

题目要求我们在每次给定一条边的权值后：
$sum\{\ max\ [\ dis(\ i,\ i\ \%\ n+1\ )\ ]\ \}$ 
（考虑边的贡献）
- 性质1：在答案统计时，每条边出现 $2$ 次（出现在 $2$ 条不同的路径里）
- 性质2：若一条边已确定值 $t_i$ ，那么其贡献为 $2×t_i$ ；若未被定值，则没办法确定其中每一条边的贡献为多少
（考虑路径的贡献）
考虑每个 $i$ 和 $i\ \%\ n + 1$ 这条路径能否定值：
- 若该路径所有点都被定值，贡献为：该路径所有 $t_i$ 的和
- 若其中存在未被定值的点，贡献为：该路径所有 $t_i$ 的和 + ( $w$ - 全图所有已定值的 $t_i$ 的和)
当你加入某一条边从权值时，他只会影响两条路径的结果。
用 set 存下每一条路径，加一条边后计算两条路径的影响即可。


```c++
#include <bits/stdc++.h>
using ll = long long;
using namespace std;

const int N = 2e5 + 100;
ll t , n , w , tot = 0;
ll cnt = 0;
ll f[21][N] , fa[N] , dep[N] , cha[N] , rd[N];
char ch[N];
bool wd[N];
set<ll> s[N];
vector<ll> cx[N];
vector<ll> G[N];

void dfs(ll x) 
{
    dep[x] = dep[f[0][x]] + 1;
    for(ll v:G[x]) 
        dfs(v);
}

void Init() 
{
    for(ll i = 1;i <= 20;i ++)
        for(ll j = 1;j <= n;j ++)
            f[i][j] = f[i - 1][f[i - 1][j]];
}

ll LCA(ll u,ll v) 
{
    if(dep[u] < dep[v]) swap(u , v);
    ll temp = dep[u] - dep[v];
    for(ll i = 20;i >= 0;i --) 
    {
        if(temp >= (1 << i)) 
        {
            temp -= (1 << i);
            u = f[i][u];
        }
    }
    if(u == v) return u;
    for(ll i = 20;i >= 0;i --) 
    {
        if(f[i][u] != f[i][v]) 
        {
            u = f[i][u];
            v = f[i][v];
        }
    }
    return f[0][u];
}

void solve() 
{
    cin >> n >> w;
    for(ll i = 1;i <= n;i ++) 
        rd[i] = 0 , G[i].clear() , wd[i] = 0 , s[i].clear() , cx[i].clear();
    for(ll i = 2;i <= n;i ++) 
    {
        ll x;
        cin >> x;
        f[0][i] = x;
        rd[x] ++;
        G[x].push_back(i);
    }
    dfs(1);
    Init();
    ll sum = 0;

    cnt = 0;
    for(ll i = 1;i <= n;i ++) 
    {
        ll nxt = i % n + 1;
        if(f[0][nxt] != i) 
        {
            cnt ++;
            ll lca = LCA(i , nxt);
            ll x = i , y = nxt;
            while(lca != x) 
            {
                s[cnt].insert(x);
                cx[x].push_back(cnt);
                x = f[0][x];
            }
            while(lca != y) 
            {
                s[cnt].insert(y);
                cx[y].push_back(cnt);
                y = f[0][y];
            }
        } 
        else 
        {
            cnt ++;
            s[cnt].insert(nxt);
            cx[nxt].push_back(cnt);
        }
    }
    
    ll gw = n;
    for(ll i = 1;i < n;i ++) 
    {
        ll x , y;
        cin >> x >> y;
        sum += y;
        for(ll v:cx[x]) 
        {
            if(s[v].size()) 
            {
                s[v].erase(x);
                if(s[v].size() == 0) gw --;
            }
        }
        cout << sum * 2 + gw * (w - sum) << ' ';
    }
    cout << "\n";
}

int main() 
{
    ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
    cin >> t;
    while(t --) solve();
    return 0;
}
```

