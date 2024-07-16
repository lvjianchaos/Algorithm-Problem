**Croc Champ 2013 - Round 2** 

**[A. Weird Game](https://codeforces.com/problemset/problem/293/A)**

<!--more-->

1 seconds / 256 megabytes

standard input / standard output

## 题目

【翻译】雅罗斯拉夫、安德烈和罗曼可以玩好几个小时的方块游戏。但游戏是三个人玩的，所以罗曼没来，雅罗斯拉夫和安德烈就玩另一个游戏。

罗曼给他们每人留了一个单词。每个单词由 2·*n* 个二进制字符"0"或"1"组成。之后，棋手开始轮流移动。雅罗斯拉夫首先移动。在移动过程中，棋手必须从 1 到 2·*n* 之间选择一个整数，这个整数在此之前没有人选择过。然后，棋手拿起一张纸，从自己的字符串中写出相应的字符。

让我们把雅罗斯拉夫的字表示为 *s* = *s*1*s*2... *s*2*n* 。同样，让我们把安德烈的字表示为 *t* = *t*1*t*2... *t*2*n* 。那么，如果雅罗斯拉夫在下棋时选择了数字 *k* ，那么他将在纸上写出字符 *s**k* 。同样，如果安德烈在下棋时选择了数字 *r* ，那么他将在纸上写出字符 *t**r* 。

当没有棋手下棋时，游戏结束。对局结束后，雅罗斯拉夫根据写在纸上的字符做出一些整数(雅罗斯拉夫可以随意排列这些字符)。安德烈也这样做。得出的数字可以包含前导零。数字最大的人获胜。如果数字相等，则游戏以平局结束。

给您两个字符串 *s* 和 *t* 。如果雅罗斯拉夫和安德烈下得很好，请确定游戏的结果。

## 输入

The first line contains integer *n* (1 ≤ *n* ≤ 106). The second line contains string *s* — Yaroslav's word. The third line contains string *t* — Andrey's word.

It is guaranteed that both words consist of 2·*n* characters "0" and "1".

## 输出

Print "First", if both players play optimally well and Yaroslav wins. If Andrey wins, print "Second" and if the game ends with a draw, print "Draw". Print the words without the quotes.

## 样例

**输入**

> 2
> 0111
> 0001

**输出**

> First

**输入**

> 3
> 110110
> 001001

**输出**

> First

**输入**

> 3
> 111000
> 000111

**输出**

> Draw

**输入**

> 4
> 01100000
> 10010011

**输出**

> Second

## 题解

**题意**

两个人各拥有一个01串a,b，长度为2n，分先后手，每一回合挑选一个最佳字符来使自己的得到1更多，挑选过字符的位置不允许再挑选。问最终谁挑选得到的1更多。

**思路**

##### 【博弈 贪心】

我们知道最终每个人手上有n个字符。观察，会有**4种**情况：

<1>同一位置，a,b串都有1；

<2>同一位置，a串有1,b串是0；

<3>同一位置，b串有1,a串是0；

<4>同一位置，a,b串都是0；

则轮到一个人的回合，他一定会先拿取<1>情况位置的字符，因为这样产生的贡献最大，可以让自己1增多，也尽可能的减少对方可选择的1的个数；

若不存在<1>的情况，他会尽可能的使自己的1增多，或使对方可选择的1减少，进而选择<2>或<3>的位置。

如果连<2><3>都选完了，剩下的<4>不产生对答案的贡献。

那么，我们该如何编写我们的代码呢？

我们知道，<1>会使贡献+1，若个数为偶数，那么两个人选择<1>的数量会相同，相当于贡献被抵消了；若为奇数，则作为先手的a会多得一个贡献，会+1。这里，我们解决了<1>对双方产生贡献的多少。

那关于<2><3>对答案的贡献如何求呢？

两个人选择<2>和<3>产生的贡献是等同的，所以最终贡献的差异体现在两个的数量之差上。假设 <2> > <3> ，则不妨设d=<2> - <3>，那么一个回合一个人选，我们要判断这时谁是先手，因为选<1>造成的。如果<1>个数是偶数，则a仍为先手，若d为偶数，则a会+d/2的贡献；若d为奇数，则a会+(d+1)/2的贡献==>都可写为(d+1)/2。其他情况类推。则最后比较两者的大小，输出即可。

## 代码

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 1e6 + 9;
// game greedy
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    // input
	int n;cin >> n;
	string a,b;cin >> a >> b;
    // 初始化
	int aa,bb,cc,ca,cb,ok;aa = bb = cc = ca = cb = ok = 0;
`	
	for(int i = 0;i < 2*n;++ i)
	{
		if(a[i] == '1' && b[i] == '1') cc ++; // <1>的数量
		if(a[i] == '1' && b[i] == '0') aa ++; // <2>的数量
		if(a[i] == '0' && b[i] == '1') bb ++; // <3>的数量
	}
	// 如果cc是奇数，则先手的a会比b多产生1个贡献，而这之后，会有b来进行选择，所以后面进行分类讨论
	if(cc&1) {ok = 1;ca += 1;}
    // cc为偶数
	if(ok == 0)
	{
		if(aa > bb) {ca += (aa - bb + 1) / 2;}
		if(aa < bb) {cb += (bb - aa) / 2;}
	}
    // cc为奇数
	else
	{
		if(aa > bb) {ca += (aa - bb) / 2;}
		if(aa < bb) {cb += (bb - aa + 1) / 2;}
	}
	// output
	if(ca > cb) cout << "First\n";
	else if(ca < cb) cout << "Second\n";
	else cout << "Draw\n";

	return 0;
}
```