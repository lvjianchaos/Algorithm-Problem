**比赛场次** 无

**比赛题目**  [P3 吸氧羊的StarryCoding之旅](https://www.starrycoding.com/problem/3)

<!--more-->

1 seconds / 128 megabytes

standard input / standard output

## 题目

吸氧羊终于注册了一个StarryCoding账号！（她很开心）

但是吸氧羊忘记了它的密码，她想起你是计算机大师，于是就来请教你。

她虽然不记得密码了，但她记得一个数组，而这个密码就是这个数组中所有区间的最大值之和。

你赶快求出来吧，她太想进去玩了！

## 输入

第一行一个整数*n*，表示数组*a*的长度。（*1≤n≤2×1e5*）

第二行*n*个整数表示数组*a*中的元素。（*1≤a[i]≤1e8*）

## 输出

一行一个整数表示结果。

## 样例

**输入**

> 5 1 1 1 1 1

**输出**

> 15

## 解释

一共有15个区间，每个区间的最大值都是1，它们的和是15。

## 题解

##### 题意

给你一数组，要求你数组中所有区间的最大值之和。

题意：给你一数组，要求你数组中所有区间的最大值之和。

##### 思路

##### 【单调栈】【贡献法】

如果我们**常规思路暴力**的话，O(n方)一定会超时。我们这样想，每个元素所“**管辖**”的区间有多少个。

从这个元素的位置，向左走，如果遇到比自己大的元素停下，记忆一下所到达的左位置，右边同理。

**单调栈**来实现，栈存储下标。                                                                                       

先从左往右遍历，若当前元素大于栈顶元素下标对应元素，则出栈，直到栈空或当前元素不大于栈顶；若栈空，此元素的左位置就是1，若不空，此元素的左位置就是stk[top]+1；将此元素入栈。

再从右往左遍历，若当前元素大于等于栈顶元素下标对应元素，则出栈，直到栈空或小于；若栈空，则右位置为n，若非空，右位置就是stk[top]-1;将此元素入栈.

最后，计算每个元素的**贡献相加**。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
ll a[N],stk[N],l[N],r[N],top;

void solve()
{
    int n;cin >> n;
    for(int i = 1;i <= n;i ++) cin >> a[i];
    
    for(int i = 1;i <= n;i ++)
    {
    	while(top && a[stk[top]] < a[i]) top --;
    	l[i] = top ? stk[top] + 1 : 1;
    	stk[++ top] = i;
    }
    
    top = 0;
    
    for(int i = n;i > 0;-- i)
    {
    	while(top && a[stk[top]] <= a[i]) top --;
    	r[i] = top ? stk[top] - 1 : n;
    	stk[++ top] = i;
    }
    
    ll ans = 0;
    for(int i = 1;i <= n;i ++) ans += (i - l[i] + 1) * (r[i] - i + 1) * a[i];
    
    cout << ans << '\n';
}
int main(void)
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    solve();
    return 0;
}
```

