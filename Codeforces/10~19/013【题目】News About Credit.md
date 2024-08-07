**比赛场次** [VK Cup 2017 - Qualification 1](https://codeforces.com/contest/769)

**比赛题目** B. News About Credit

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

波利卡普在大学学习，他所在的小组由 *n* 名学生(包括他自己)组成。他们都在社交网络 "TheContacnt!"上注册。

并非所有学生都同样善于交际。您可以知道每个学生的值 *a[i]* —— *i-th* 同意每天发送信息的最大数量。学生不能给自己发送信息。

清晨，波利卡普知道了一个重要消息，明天将是编程学分。因此，有必要通过私人消息紧急通知所有组员。

您的任务是制定一个使用私人信息的计划，以便

- 学生 *i* 发送的消息不超过 *a[i]* (从 1 到 *n* 的所有 *i* )；
- 所有学生都知道学分的消息(最初只有波利卡普知道)；
- 学生只有在自己知道的情况下才能通知其他学生。

让我们假设所有学生的编号都是从 1 到 *n* 的不同数字，而波利卡普**的编号始终是 1 。在这项任务中，你不应该最小化消息的数量、时间、所有人知道信用的时间或其他参数。找到满足上述要求的任何使用私人信息的方法。**

## 输入

第一行包含正整数 *n* ( 2 ≤ *n* ≤ 100 ) - 学生人数。

第二行包含序列 *a*1, *a*2, ..., *an* ( 0 ≤ *ai* ≤ 100 )，其中 *ai* 等于第 *i* 位学生同意发送的最大信息数。考虑到波利卡普 **总是**有 1 这个数字。

## 输出

如果无法通知所有学生学分，则在第一行打印-1。

否则，在第一行打印整数 *k* - 将发送的信息数。在接下来的每行 *k* 中，打印两个 **不同的**整数 *f* 和 *t* ，表示编号为 *f* 的学生向编号为 *t* 的学生发送了消息。所有信息都应按时间顺序打印。这意味着发送消息的学生必须已经知道这条消息。我们假定学生可以接收到重复的学分消息。

如果有多个答案，可以打印任何一个答案。

## 样例

**输入**

> 4
> 1 2 1 0

**输出**

> 3
> 1 2
> 2 4
> 2 3

**输入**

> 6
> 2 0 1 3 2 0

**输出**

> 6
> 1 3
> 3 4
> 1 2
> 4 5
> 5 6
> 4 6

**输入**

> 3
> 0 2 2

**输出**

> -1

## 题解

##### 【贪心】【排序】

蒟蒻WA了一发才做对，没有考虑到此种**输入**的情况：

> 3
>
> 1 0 1

观察数据范围，我们考虑暴力。如何暴力？

我们可以这样**贪心**。将除下标1也就是`Polycarp`的人按照可发消息数从大到小**排序**。再双重`for循环`（for循环内部具体看代码），我们肯定是想<u>把前面的多的消息数给使用完再下一个人使用</u>，我们记录哪些人知道消息，若知道就下一个；注意计数，且利用`vector`存储答案。还要判断最后是否真的能发送给所有人。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

struct INFORM
{
	int i; // 下标
	int a; // 可发送消息数
	bool is = false; // 是否知道消息
};
// 重载<
bool operator < (INFORM x,INFORM y)
{
	if(x.a == y.a) return x.i > y.i;
	return x.a > y.a;
}

int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	// init and input
    int n;cin >> n;
	INFORM a[n + 9];
	for(int i = 1;i <= n;i ++) cin >> a[i].a,a[i].i = i;
	a[1].is = true;
    // to solve
	sort(a + 2,a + n + 1); // 排序
	int cnt = 0; // 总发送的消息数
	vector<pair<int,int>> ans; // 存储答案：'谁first =》谁second'发了消息
	for(int i = 1;i <= n;i ++)
	{
		for(int j = i + 1;j <= n;j ++)
		{
			if(a[i].a <= 0) break; // 若到达发送消息数上限
			if(a[j].is == true) continue; // 若一已经知道消息
			ans.push_back({a[i].i,a[j].i}); // 答案推进去
			cnt ++; // 计数++
			a[j].is = true; // 将此人的知道消息改为是
			a[i].a --; // 可发消息数--
		}
	}
	int ok = 1; // 判断是否可使所有人知道
	for(int i = 1;i <= n;i ++)
	{
		if(a[i].is == false) 
		{
			ok = 0;
			break;
		}
	}
    // output
	if(ok)
	{
		cout << cnt << '\n';
	for(auto &i : ans) cout << i.first << ' ' << i.second << '\n';
	}
	else
	{
		cout << -1 << '\n';
	}
	return 0;
}
```

