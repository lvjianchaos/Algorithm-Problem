
$time\ limit\ per\ test:\ 2\ second$
$memory\ limit\ per\ test:\ 256\ megabytes$
$input:\ standard\ input$
$output:\ standard\ output$

**题意**：

有一个长度为 $n$ 的01字符串 $s$ , 问是否能组成 **美丽方阵**
美丽方阵满足以下条件：
- 长、宽相等
- 四周也就是最外圈为 $1$ ，内部都为 $0$ 

**题解**：

【暴力】
首先，判断长度是否为平方数，若不是则不能；
其次，按照 $sqrt(n)$ ，$for$ 循环嵌套判断即可。

```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

void solve()
{
    int n;cin >> n;
    int p = sqrt(n);
    if(p * p != n) 
    {
    	string s;cin >> s;
    	cout << "No\n";
    }
    else
    {
    	char m[p+2][p+2];
    	for(int i = 1;i <= p;i ++)
    		for(int j = 1;j <= p;j ++)
    			cin >> m[i][j];
    	for(int i = 1;i <= p;i ++)
    	{
    		for(int j = 1;j <= p;j ++)
    		{
    			if(i == p || j == p || i == 1 || j == 1) 
    			{
    				if(m[i][j] == '1') continue;
    				else { cout << "No\n"; return; }
    			}
    			else
    			{
    				if(m[i][j] == '0') continue;
    				else { cout << "No\n"; return; }
    			}
    		}
    	}
    	cout << "Yes\n";
    	return;
    }
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int t;cin >> t;
    while(t --) solve();
    return 0;
}
```
