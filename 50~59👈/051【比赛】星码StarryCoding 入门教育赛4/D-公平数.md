**比赛场次** 星码StarryCoding 入门教育赛4

**比赛题目** D

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

如果一个正整数能被它的每个位上的非零数字整除，我们就称它为公平数。例如102是一个公平数（因为它可以被2和8整除），但282不是（因为它不能被8整除）。

现在给定一个正整数𝑛，求最小的一个正整数𝑥满足以下条件：

- 𝑥≥𝑛；
- 𝑥是一个公平数。

## 输入

第一行包含一个整数𝑇(1≤𝑇≤1000),表示测试用例组数。
接下来𝑇行每行都包含一个正整数𝑛(1≤𝑛≤1e18)。

## 输出

每个测试用例打印一个整数。

## 样例

**输入**

> 4 
>
> 1 
>
> 282 
>
> 1234567890 
>
> 1000000000000000000

**输出**

> 1 
>
> 288 
>
> 1234568040 
>
> 1000000000000000000

## 题解

【**数学**】【gcd】【lcm】

如果一个正整数能被它每一位上的非零数字整除，那么**这个正整数也能被它的每一位上非零数字的𝐿𝐶𝑀整除**。

而𝐿𝐶𝑀(1...9)=2520,所以符合条件的解一定不超过𝑛+2520

所以我们依次枚举比𝑛大的每一个数并判断是否符合条件即可。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
// gcd()模板
int gcd(int a,int b){return b == 0 ? a : gcd(b,a % b);}
int lcm(int a,int b){return a * b / gcd(a,b);}

bool check(int n)
{
    int x = n;
    while(x)
    {
        int k = x % 10;
        if(k != 0)
        {
            if(n % k != 0) return false;
        }
        x /= 10;
    }
    return true;
}

void solve()
{
    int n;cin >> n;
    while(!check(n)) n ++;
    cout << n << '\n';
}

int main()
{
    int t;cin >> t;
    while(t --) solve();
}
```

