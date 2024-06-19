**比赛场次** [牛客周赛 Round 46](https://ac.nowcoder.com/acm/contest/84444)

**比赛题目** [E-立希喂猫](https://ac.nowcoder.com/acm/contest/84444/E)

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

Taki买了 n 种猫粮，第 i 种猫粮的营养值为 ai 、数量为 bi 。

 猫猫的饭量是无穷的，每一天她可以吃任意数量的猫粮，但是同一种猫粮她一天只会吃一次。

 Taki想知道在 k 天内，猫猫可以获得的最大营养值之和是多少。

## 输入

第一行输入一个正整数 n(1≤n≤1e5)，表示猫粮种数。

第二行输入 n 个正整数 ai(1≤ai≤1e4)，表示每种猫粮的营养值。

第三行输入 n 个正整数 bi(1≤bi≤1e9)，表示每种猫粮的数量。

第四行输入一个正整数 q(1≤q≤1e5)，表示询问次数。

接下来 q 行，每行输入一个正整数 k(1≤k≤1e9)，表示询问的天数。

## 输出

对于每个询问，在一行中输出一个整数表示答案

## 样例

**输入**

> 3
> 3 7 5
> 1 2 1
> 3
> 1
> 2
> 3

**输出**

> 15
> 22
> 22

## 解释

一种可行的方案是：
第 1 天：
猫猫吃第1、2、3种猫粮，营养值之和为3+7+5=15。
第 2 天：
猫猫吃第2种猫粮，营养值之和为7。
第 3 天：
猫猫不吃猫粮。

## 题解

**【二分】【前缀和】**

首先，由题意得：猫猫每天可以吃无限种猫粮。

一种**贪心**的思想：k天内为了使营养值最大，当然是让猫猫每天都吃所有种类的猫粮直到该种类猫粮没有了。

所以猫粮只要数量>=k的，那么就可以每天都吃，即营养值是a[i] * k；只要数量<k的，那么最多吃该猫粮数量a[i]，即营养值为a[i] * b[i]。

那么我们就会去想怎么找出有哪些猫粮数量小于k，又有哪些猫粮数量>=k?

这样的查找一个中间值<=k，我们很自然想到对猫粮的数量进行**二分搜索**。而其准备工作就是**排序**。

对于其中的细节，我们使用**前缀和和后缀和**来使查询时快速获得答案。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
struct catFood
{
    ll a,b; // 分别表示营养值和数量
};

ll k;
ll pre[N],suff[N];
catFood v[N];
// 排序，数量少的在前面
bool cmp(catFood x,catFood y)
{
    if(x.b == y.b) return x.a > y.a;
    return x.b < y.b;
}
// 二分中check()判断函数
bool check(ll mid)
{
    if(v[mid].b <= k) return true;
    return false;
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    // input
    int n;cin >> n;
    for(int i = 1;i <= n;i ++) cin >> v[i].a;
    for(int i = 1;i <= n;i ++) cin >> v[i].b;
	// sort 
    sort(v + 1,v + 1 + n,cmp);
    // prefix and suffix of a
    for(int i = 1;i <= n;i ++) pre[i] = pre[i - 1] + v[i].b * v[i].a;
    for(int i = n;i > 0;i --) suff[i] = suff[i + 1] + v[i].a;
    // qustion
    int q;cin >> q;
    while(q --)
    {
		// binary search
        cin >> k;
        ll l = 0,r = n;
        while(l + 1 != r)
        {
            ll mid = (l + r) / 2;
            if(check(mid)) l = mid;
            else r = mid;
        }
        if(v[r].b <= k) l = r;
        // output
        cout << pre[l] + k * suff[l + 1] << '\n';
    }
    return 0;
}
```

