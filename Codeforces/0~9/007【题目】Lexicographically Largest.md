**比赛场次** [think-cell Round 1](https://codeforces.com/contest/1930)

**比赛题目** C. Lexicographically Largest

<!--more-->

2 seconds / 256 megabytes

standard input / standard output

---
## 题目

Stack has an array a of length n. He also has an empty set S. Note that S is **not** a multiset.

He will do the following three-step operation exactly n times:

1. Select an index i such that 1≤i≤|a|.
2. Insert†† ai+i into S.
3. Delete ai from a. Note that the indices of all elements to the right of ai will decrease by 11.

Note that after n operations, a will be empty.

Stack will now construct a new array b which is S **sorted in decreasing order**. Formally, b is an array of size |S| where bi is the i-th largest element of S for all 1≤i≤|S|.

Find the lexicographically largest **b** that Stack can make.

 **A** set can only contain unique elements. **Inserting an element that is already present in a set will not change the elements of the set.**

‡‡ An array p is lexicographically larger than a sequence q if and only if one of the following holds:

- q is a prefix of p, but p≠q; or
- in the first position where p and q differ, the array p has a larger element than the corresponding element in q.

Note that [3,1,4,1,5]is lexicographically larger than [3,1,3], [][], and [3,1,4,1] but not [3,1,4,1,5,9], [3,1,4,1,5], and [4].

## 输入

Each test contains multiple test cases. The first line contains a single integer t (1≤t≤1e4) — the number of test cases. The description of the test cases follows.

The first line of each test case contains a single integer n (1≤n≤3e5) — the length of array a.

The second line of each test case contains n integers a1,a2,…,an (1≤ai≤1e9) — the elements of array a.

The sum of n over all test cases does not exceed 3⋅1053⋅105.

## 输出

For each test case, output the lexicographically largest b.

## 样例

**输入**

> 3
> 2
> 2 1
> 5
> 1 100 1000 1000000 1000000000
> 3
> 6 4 8

**输出**

> 3 2 
> 1000000005 1000004 1003 102 2 
> 11 7 6 

## 解释

In the first test case, select i=1 in the first operation, insert a1+1=3 in S, and delete a1from a. After the first operation, a becomes a=[1]. In the second operation, we select i=1 again and insert a1+1=2 in S. Thus S={2,3}, and b=[3,2].

Note that if you select i=2 in the first operation, and i=1 in the second operation, S={3} as 3 will be inserted twice, resulting in b=[3].

As [3,2] is lexicographically larger than [3], we should select i=1 in the first operation.

In the second test case, in each operation, select the last element.

## 题解

##### 【贪心 思维】

##### 题意

给你一个数组，数组中每个元素的实际的值是索引加该元素本身的值。如果我们选择某个元素，那么会使这个元素右边的所有元素的值都减1，因为索引会减一。问我们如何才能使新构建的集合中词序最大。

##### 思路

为了尽可能**避免索引减一**，我们一定会想**从右往左**的进行insert操作，但不允许存在重复元素，所以如果有重复元素，为了**不减少元素数量**，就先insert左边的，再insert右边的。

## 代码


```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
// 先将 a[i] + i 从大到小排序，
// 然后顺序输出（对应原序列的操作就是从右到左取数加入集合，这样损失是最小的）。
// 注意输出时若当前数和上一个数相同，
// 为避免被去重则需要减一（对应原序列的操作即先取较左边的数，再取较右边相同的数）。
const int N = 3e5 + 9;
vector<ll> a;
 
void solve()
{
	a.clear();
	int n;cin >> n;
	for(int i = 1;i <= n;i ++)
	{
		int x;cin >> x;
		x += i;
		a.push_back(x);
	}
	sort(a.begin(),a.end(),greater<ll> ());
	for(int i = 1;i < n;i ++) a[i] = min(a[i],a[i - 1] - 1); // 核心代码
	for(auto &i :a) cout << i << ' ';
	cout << '\n';
	return;
}
 
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int _ = 1;cin >> _;
	while(_ --) solve();
	return 0;
}
```

### 