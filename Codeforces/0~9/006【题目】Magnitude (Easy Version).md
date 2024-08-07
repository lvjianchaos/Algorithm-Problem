**比赛场次 [Codeforces Global Round 26](https://codeforces.com/contest/1984)**

**比赛题目 C1. Magnitude (Easy Version)**

<!--more-->

2 seconds / 256 megabytes

standard input / standard output

## 题目

**两个版本的问题不同。您可能需要同时阅读两个版本。只有两个版本的问题都解决了，你才能进行破解。**

给你一个长度为 n 的数组 a。从 c=0开始。然后，对从 1 到 n 的每个 i (按递增顺序)做以下选项中的一个：

- 选项 1 ：将 c 设置为 c+a[i] 。
- 选项 2 ：将 c 设为 |c+a[i]| ，其中 |x| 是 x 的绝对值。

假设 c 经过上述过程后的最终最大值等于 k 。求出 k .

## 输入

第一行包含一个整数 t ( 1≤t≤1e4 )--测试用例数。( 1≤t≤1e4) - 测试用例的数量。

每个测试用例的第一行包含一个整数 n ( 2≤n≤2e5)。

每个案例的第二行包含 n 个整数 a[1] 、 a[2] 、 a[3] 、 …… 、 a[n] ( −1e9≤a[i]≤1e9)。

所有测试用例中 n 的总和不超过 3e5 。

## 输出

对于每个测试用例，输出一个整数 —— k 的值。

## 样例

**输入**

> 5
> 4
> 10 -9 -3 4
> 8
> 1 4 3 4 1 4 3 4
> 3
> -1 -2 -3
> 4
> -1000000000 1000000000 1000000000 1000000000
> 4
> 1 9 8 4

**输出**

> 6
> 24
> 6
> 4000000000
> 22

## 解释

在第一个测试案例中，如果我们每次都将 c 设为绝对值，那么最终结果就是 6 。可以证明这是最大结果。

在第二个测试案例中，取绝对值永远不会改变任何结果，因此我们可以不做任何操作，直接对数组求和，得到 24。

在第三个测试案例中，我们最好等到最后才将 c 设为绝对值，从而得到 6 的答案。

## 题解

##### 【前缀和 贪心 暴力】

**问题的关键**在于我们何时选择`option 1`，何时选择`option 2`？分别选择几次？

显而易见，我们选择`option 2`的时候一定是此时的`c+a[i] < 0`，因为如果此时`c+a[i]`加`||`效果等同于选择`option 1`。

然后我们思考，我们要选择几次`option 2`？我们来思考**最后两次**选择`option 2`的情形。

我们会发现，`选项 2`只可选择1次。不严谨的**证明**：假设我们在进行最后一次`option 2`，说明之前的倒数第二次`c+a[i] < 0`，那么最后一次时，`c+a[i]`又变得小于0。模拟这样的过程：我们不妨设之前的`c+a[i] = t(t < 0)`，然后从此处到最后一次选择选项2的其间过程又总共`+ sum(- t + sum > 0)`，最后`+a[i](-t + sum + a[i] < 0)`；但若倒数第二次不选择`option 2`，我们会得到`t + sum + a[i] < -t + sum + a[i] < 0`。既然如此，我们为什么要大费周章地在倒数第二次选项2呢？我们只需要**在某处进行一次选项2的操作即可**。利用**前缀和**，然后暴力会得到答案。

## 代码

```c++
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

const int N = 2e5 + 9;
ll a[N],pre[N];
void solve()
{
	// input
	int n;cin >> n;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	// solve
	// cal the prefix of array a
	for(int i = 1;i <= n;i ++) pre[i] = pre[i - 1] + a[i];
	
	ll ans = 0;
	for(int i = 0;i <= n;i ++)
		ans = max(ans,abs(pre[i]) + pre[n] - pre[i]);
	// output
	cout << ans << '\n';
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;cin >> t;
	while(t --) solve();
	return 0;
}
```

