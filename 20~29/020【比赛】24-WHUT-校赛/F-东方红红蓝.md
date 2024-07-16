**比赛场次**

**比赛题目**

<!--more-->

2 seconds / 512 megabytes

standard input / standard output

## 题目

Yaimsea 正在游玩东方星莲船，他收集了 a 个红色飞碟，b 个蓝色飞碟和 c 个绿色飞碟，他可以通过消耗同一颜色的 3 个飞碟或者红色、蓝色和绿色飞碟各 1 个的方式来合成 1 个UFO。

 Yaimsea 想知道他最多能合成出几个UFO。

## 输入

一行三个整数 a,b,c(0 ≤ a,b,c ≤ 1e9)，表示红蓝绿飞碟的数量。

## 输出

一行一个整数，表示最多能合成出的 UFO 的数量。

## 样例

**输入**

> 1 3 3

**输出**

> 2

## 题解

#### 赛时有猜的成分

简单的说，思路就是：

1. 先进行完**异色合成**操作，再进行完**同色合成**操作所得答案；
2. 先进行完**同色合成**操作，再进行完**异色合成**操作所得答案。

取两者**最大**即可。

#### 官方题解

可以发现 3 次异色合成可以等效替代为 3 次同色合成(每个颜色各 1 次)，因此所有的方案都可以替换为异色合成次数小于 3 的方案。

 从 0 到 2 枚举异色合成的次数，对每一个颜色的剩余飞碟尽可能多地使用同色合成就能找到答案。 

本题的颜色只有 3 种，一些其他的做法也是能过的，如一直进行异色合成直到有一个 颜色的飞碟耗尽，如果剩余飞碟数量分布是 0,2,2 则反悔 1 次进行 2 次同色合成等做法。

## 代码

##### **赛时AK代码**

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    ll a,b,c;cin >> a >> b >> c;
    
    ll mi = min(a,min(b,c));
    ll ans_1 = mi + (a - mi) / 3 + (b - mi) / 3 + (c - mi) / 3;
    ll ans_2 = a / 3 + b / 3 + c / 3 + min(a%3,min(b%3,c%3));
    ll ans = max(ans_1,ans_2);
    cout << ans << '\n';
    return 0;
}
```

##### 官方题解代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    
    ll a,b,c;cin >> a >> b >> c;
    ll mi = min(a,min(b,c));
    ll ans = 0;
    
    for(int i = 0;i < 3;i ++)
    {
        ll num = mi - i;
        num = max(0ll,num); // 注意
        ll a_cp = a - num;
        ll b_cp = b - num;
        ll c_cp = c - num;
        
        ans = max(mi - i + a_cp / 3 + b_cp / 3 + c_cp / 3,ans);
    }
    
    cout << ans << '\n';
    return 0;
}
```

