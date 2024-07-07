**比赛场次**

**比赛题目**

<!--more-->

2 seconds / 512 megabytes

standard input / standard output

## 题目

已知，在 C++ 语言中，调用某类的构造函数时，将先按照继承时声明的顺序依次调用该类的所有父类的构造函数，之后执行该类本身的构造函数的函数体。

 现有某生在学习面向对象程序设计课程的过程中编写了一程序，其中共包含 n 个类，依次编号为 1∼n。每个类均可能是一个基类，或继承自若干个父类。每个类的构造函数体中仅包含输出该类编号的一条输出语句。

 现在，请你设计程序，计算：若调用第 n 个类的构造函数，则程序产生的输出中第 m 个类的编号是什么。

## 输入

第一行一个整数 n(1≤n≤1e5)，表示该生设计的程序中类的数量。

接下来 n 行，第 i 行首先输入一个非负整数 k，表明第 i 号类的父类个数。接下来 k 个整数 f1,f2,…,fk，依次为该类的所有父类编号。

最后一行一个整数 m(1≤m≤1e15)，意义见题目描述。

保证 1≤fx<i，且 fx 互不相同。保证 ∑k≤1e6。保证程序产生的输出中至少包含 m 个类的编号。

## 输出

一个正整数，表示所求的答案。

## 样例

**输入**

> 4
> 0
> 1 1
> 2 2 1
> 2 1 3
> 5

**输出**

> 3

## 解释

该生设计的程序示例如下：

```c++
#include <bits/stdc++.h> 
class Class1 
{ 
public: 
    Class1() { std::cout << 1 << " "; }
}; 
class Class2 : Class1 
{ 
public: 
    Class2() { std::cout << 2 << " "; }
}; 
class Class3 : Class2, Class1 
{ 
public: 
    Class3() { std::cout << 3 << " "; }
}; 
class Class4 : Class1, Class3 
{ 
public: 
    Class4() { std::cout << 4 << " "; }
}; 
int main() 
{ 
    Class4 object; 
    return 0;
}
```

## 题解

##### 官方题解

可以预处理出 c[i] 表示调用第 i 个类的构造函数得到的输出长度。 

考虑调用第 u 个类的构造函数。依次遍历其每个父类 v，若 k ≤ c[i]，则将 u 移动到 v。 否则，将 k 减去 c[i]。 

若该过程结束，说明调用的就是点 u 的构造函数。

实现时可能需要注意将 c[i] 和 INF 取 min。



就是类似于DFS回溯，并利用了记忆化搜索。

如下图![image-20240708010522183](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240708010522183.png)

## 代码

##### 赛时AK代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;

vector<ll> v[N]; // 邻接表
ll mem[N]; // 记忆数组
bool ok = 0;
ll m,ind;

void dfs(ll n)
{
    // 遍历该节点的所有父节点
    for(auto & i : v[n])
    {
        // 如果搜索到底，就返回（v[1] == 0）
        if(i == 0) return;
        // 如果已搜索到并输出，就返回
        if(ok) return;
        // 如果此节点被记忆过
        if(mem[i])
        {
            // 判断是否m在这段里，若不在，可优化
            if(ind + mem[i] < m)
            {
                ind += mem[i];
                continue;
            }
        }
        
        ll last = ind;
        
        dfs(i);
        
        ind ++;
        
        if(!mem[i])
        {
            mem[i] = ind - last;
        }
        // 找到就输出，并返回
        if(ind == m) 
        {
            cout << i << '\n';
            ok = 1;
            return;
        }
    }
    
    return;
}

void solve()
{
    int n;cin >> n;
    // build the graph
    for(int i = 1;i <= n;i ++)
    {
        int k;cin >> k;
        while(k --)
        {
            ll f;cin >> f;
            v[i].push_back(f);
        }
    }
    cin >> m;
    dfs(n);
    if(ind == m - 1) cout << n << '\n';
    return;
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int _ = 1;
    while(_ --)
    {
        solve();
    }
    return 0;
}
```

