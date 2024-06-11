**比赛场次** 牛客小白月赛91

**比赛题目** [Bingbong的奇偶世界](https://ac.nowcoder.com/acm/contest/78807/D)

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

Bingbong有一个长度为n 的数字字符串S，该字符串仅包含[0,9]的数字。

Bingbong想要从中挑选出若干个字符，然后按照其相对顺序重新拼接而成一个数字，使其是一个**无前导**0**的偶数**。

 例如：当n=3,S=100。 其包含的偶数数字有0,0,10,10,100。而00是不符合条件的，因为其含有前导0。

由于字符串实在是太长了，他一个人数不过来，请您帮他计算一下该字符串中含有的偶数方案总数， 结果对1e9+7取模。

## 输入

第一行一个整数n(1≤n≤2×1e5)，表示字符串S的长度。

第二行一个长度为n的字符串S,保证输入只含[0,9]的数字。

## 输出

一个整数，表示最后含有的偶数方案总数。

## 样例

**输入**

> 3
>
> 100

**输出**

> 5

**输入**

> 5 
>
> 12345

**输出**

> 10

## 题解

##### 题意

给定[0~9]字符串，要求按顺序能组成多少个偶数，注意：不存在前导零。

##### 思路

**【dp思想】**

能组成多少个偶数，即最后一位为[0,2,4,6,8]，我们**顺序遍历**，求**此位之前**的字符串能组成多少个合法的数。

​			设此为之前能有cnt个合法数。

​			若当前数是0，那么我们会将0放置在末尾，答案+cnt，并且0可单独成数，故再+1；合法数cnt变为2*cnt;

​			若当前数是2，4，6，8.那么我们的答案+cnt+1；合法数cnt * 2 + 1;

​			若当前数是1，3，5，7，9，那么我们的cnt  * 2 + 1；

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const ll MOD = 1e9 + 7;

int main(void)
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    
    int n;string s;
    cin >> n >> s;
    
    ll cnt = 0,ans = 0;
    for(int i = 0;i < n;i ++)
    {
        if(s[i] == '0')
        {
            ans = (ans + cnt + 1) % MOD;
            cnt = cnt * 2 % MOD;
        }
        else if((s[i] - '0') % 2 == 0)
        {
            ans = (ans + cnt + 1) % MOD;
            cnt = (cnt * 2 + 1) % MOD;
        }
        else
        {
            cnt = (cnt * 2 + 1) % MOD;
        }
    }
    
    cout << ans << '\n';
    
    return 0;
}
```

### 