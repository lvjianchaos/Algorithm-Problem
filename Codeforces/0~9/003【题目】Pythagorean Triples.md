**[Codeforces Round 368 (Div. 2)](https://codeforces.com/contest/707)**

 **[C. Pythagorean Triples](https://codeforces.com/contest/707/problem/C)**

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

Katya studies in a fifth grade. Recently her class studied right triangles and the Pythagorean theorem. It appeared, that there are triples of positive integers such that you can construct a right triangle with segments of lengths corresponding to triple. Such triples are called *Pythagorean triples*.

For example, triples (3, 4, 5), (5, 12, 13) and (6, 8, 10) are Pythagorean triples.

Here Katya wondered if she can specify the length of some side of right triangle and find any Pythagorean triple corresponding to such length? Note that the side which length is specified can be a cathetus as well as hypotenuse.

Katya had no problems with completing this task. Will you do the same?

## 输入

The only line of the input contains single integer *n* (1 ≤ *n* ≤ 1e9) — the length of some side of a right triangle.

## 输出

Print two integers *m* and *k* (1 ≤ *m*, *k* ≤ 1e18), such that *n*, *m* and *k* form a Pythagorean triple, in the only line.

In case if there is no any Pythagorean triple containing integer *n*, print - 1 in the only line. If there are many answers, print any of them.

## 样例

**输入**

> 3

**输出**

> 4 5

**输入**

> 1

**输出**

> -1

**输入**

> 17

**输出**

> 144 145

**输入**

> 67

**输出**

> 2244 2245

## 题解

**题意**

给你一个整数n，让你构造另两个整数满足三角形的勾股定理。

**思路**

##### 【**构造**】

首先，`n <= 2`无解

由题意得：$a^2 + b^2 = c^2$；不妨设`n = a`，则$n^2 = c^2 - b^2 = (c-b) * (c+b)$；

有`n=1 * n ==> c-b=1`，$c + b = n ^ 2$；解得：$b=(n^2-1)/2 \ \  c=(n^2+1)/2$

这样一组构造只满足n为**奇数**；

那么如何构造**偶数的情形**呢？

有`n=n/2 * 2 ==> c-b=2`，$c+b=n^2/2$；解得：$b=n^2/4-1\ \ c=n^2/4+1$

这就是偶数的情况。

## 代码

```c++
#include <bits/stdc++.h>
using ll = long long;
using namespace std;

int main(){
   ll n;cin >> n;
   if(n <= 2) return cout << -1,0;
   if(n&1) cout << (n * n + 1) / 2 << ' ' << (n * n - 1) / 2 << '\n';
   else cout << (n * n) / 4 + 1 << ' ' << (n * n) / 4 - 1 << '\n';
   return 0;
}
```

​		