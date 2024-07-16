**比赛场次** [Codeforces Round 952 (Div. 4)](https://codeforces.com/contest/1985)

**比赛题目** F. Final Boss

<!--more-->

2 seconds / 256 megabytes

standard input / standard output

## 题目

您正在面对您最喜欢的视频游戏中的最终 Boss。敌人的生命值为 hℎ 。你的角色有 n 次攻击。 i-th 的攻击会对敌人造成 a[i] 的伤害，但冷却时间为 c[i] 个回合，也就是说，如果你当前的回合是 x ，那么下一次使用该攻击的时间就是 x+c[i] 个回合。每个回合，你都可以使用当前未冷却的所有攻击**一次**。如果所有攻击都处于冷却状态，则该回合你什么也不做，跳到下一回合。

最初，所有攻击都不在冷却时间内。要花多少回合才能打败BOSS？当 BOSS 的生命值为 0 或更低时，它就被击败了。

## 输入

第一行包含 t ( 1≤t≤1e4 ) - 测试用例数。

每个测试用例的第一行包含两个整数 ℎ 和 *n* ( 1≤h,n≤2e5)，分别是 BOSS 的健康值和你的攻击次数。

每个测试用例的下一行包含 n 个整数 a1,a2,...,an( 1≤ai≤2e5 ) - 你的攻击伤害。

每个测试用例的下面一行包含 n 个整数 c1,c2,...,cn( 1≤ci≤2e5 ) - 攻击冷却时间。

保证所有测试用例中的 ℎ 和 *n* 之和不超过 2e5 。

## 输出

对于每个测试用例，输出一个整数，即打败BOSS所需的最少回合数。

## 样例

**输入**

> 8
> 3 2
> 2 1
> 2 1
> 5 2
> 2 1
> 2 1
> 50 3
> 5 6 7
> 5 6 7
> 50 3
> 2 2 2
> 3 3 3
> 90000 2
> 200000 200000
> 1 1
> 100000 1
> 1
> 200000
> 6 7
> 3 2 3 2 3 1 2
> 6 5 9 5 10 7 7
> 21 6
> 1 1 1 1 1 1
> 5 5 8 10 7 6

**输出**

> 1
> 3
> 15
> 25
> 1
> 19999800001
> 1
> 21

## 解释

**注**

在第一个测试案例中，你可以在第一回合使用攻击 $$$1$$$ 和 2 ，总共造成 3 伤害，并杀死 BOSS。

在第二种情况下，使用以下攻击可以在 3 个回合内击败 BOSS：

第 1 个回合：使用攻击 1 和 2 ，对BOSS造成 3 伤害。现在 BOSS 的生命值还剩 2 。

转向 2 ：使用攻击 2 ，对BOSS造成 1 伤害。现在BOSS的生命值剩余 1 。

回合 3 ：使用攻击 1 ，对BOSS造成 2 伤害。现在BOSS的生命值还剩 -1 。由于它的生命值小于等于 0 ，所以你击败了BOSS。

第六个测试案例：请记住使用 64 位整数，因为答案可能会很大。

## 题解

##### 【优先队列】【重载<运算符】

显而易见，我们不能就一回合一回合判断是否有技能攻击，这样一定超时。

我们观察h的范围<=2e5，这样，我们<u>一定能够在<=h的攻击次数下即最多操作2e5次就可击败Boss</u>。

所以我们可以使用**优先队列**，**关键点**是一上来把攻击全用上去，然后等cd，谁好用谁，注意每次加cd，这样就🆗了，关键是优先队列。

细节可看**代码注释**。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;
ll a[N]; // 伤害
ll c[N]; // 冷却回合数
// 攻击
struct ATTACK
{
	ll turn; // 轮次
	ll i; // 编号
};
// 重载'<'
bool operator < (const ATTACK x,const ATTACK y)
{
	if(x.turn == y.turn) return a[x.i] < a[y.i]; // 若turn相等，a[i]大的在前
	return x.turn > y.turn; // turn小的在前
}

void solve()
{
	// input
	int h,n;cin >> h >> n;
	for(int i = 1;i <= n;i ++) cin >> a[i];
	for(int i = 1;i <= n;i ++) cin >> c[i];
	//优先队列
	priority_queue<ATTACK> pq;
	// 初始化
	for(int i = 1;i <= n;i ++)
		pq.push({1,i});
	
	ll ans = 0; // 答案，即循环结束时的攻击的回合 
	while(h > 0) // 条件：boss血量>0
	{
		int i = pq.top().i; // 此次攻击的编号
		ll t = pq.top().turn + c[i]; // 这个编号的攻击下一次施展的回合
		ll hurt = a[i]; // 此次攻击的伤害
		h -= hurt;
		ans = pq.top().turn; // 更新一下答案
		pq.pop(); // 弹出使用过的攻击
		pq.push({t,i}); // 将下一次的此编号的攻击入队
	}
	// output
	cout << ans << '\n';
}

int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int _ = 1;cin >> _;
	while(_ --) solve();
	return 0;
}
```