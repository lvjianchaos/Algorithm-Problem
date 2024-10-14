
**题目：**

Alice 和 Bob 操纵一个棋子在棋盘上移动：
- 初始位置——（0，0）
- 棋子只能向 **右** 或 **下** 移动，每次移动一步，即: （ i , j ）=>（ i + 1 , j ）/（ i ，j + 1 ）
- 有一终点（ x , y ），在谁的回合到达即判为获胜；若都不能走到则判为平局
- Alice 先手，双方均采取最优决策（尽可能让自己赢，自己赢不了则尽可能平局）
Alice获胜输出“YES”；Bob获胜输出“NO”；平局输出“PING”.

**题解：**

【思维】【游戏】
由于每人一次移动一个单位距离，所以：
- 曼哈顿距离为 **奇数** , Bob一定不会赢。若 x 和 y 相差 1 ，那么 Alice 获胜，因为 Bob 不能 x 和 y 的范围  ；否则 PING！
- 曼哈顿距离为 **偶数**，Alice一定不会赢。若 x 和 y 相等，那么 Bob获胜，理由同上；否则 PING!
(当不会赢时，一定尽可能的希望平局才为最优)
另外需要注意的是 **终点** 的（ x , y ）一定不会是负数。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

void solve()
{
    int x,y;cin >> x >> y;
    int sum = x + y;
    if(x < 0 || y < 0) return cout << "PING\n",void();
    if(sum & 1)
        if(abs(x - y) == 1) return cout << "YES\n",void();
        else return cout << "PING\n",void();
    else
        if(x == y) return cout << "NO\n",void();
        else return cout << "PING\n",void();
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}
```
