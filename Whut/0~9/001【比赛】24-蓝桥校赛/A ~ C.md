>[蓝桥杯校赛](https://pintia.cn/problem-sets/1864140251970043904/exam/overview) 
## A 孤独的精灵
### 题意：
输入 n 个数，其中的数都成对出现，但有一个例外也就是”孤独的数“，请你输出这个数。
### 题解：
#### ***暴力***
使用`unordered_map`，存储某个数出现的次数，之后再遍历出现的元素种类，判断其出现次数，如有$1$，直接输出即可。

这是有一定几率可以卡过测试点，也本就不是优秀的写法。
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
const int N = 2e6 + 9;
unordered_map<int,int> mp; // 存数字出现次数
vector<int> a; // 存出现的数字种类
int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int n,x;cin >> n;
    for(int i = 0;i < n;i ++) {
        cin >> x;
        if(!mp[x]) a.push_back(x); 
        mp[x] ++;
    }

    for(auto &i : a)
    	if(mp[i] == 1) 	
    		return cout << i << '\n', 0;

    return 0;
}
```
##### 关于使用`unordered_map`而不使用`map`：
***1. 底层实现***
- **`map`**：
    - 底层实现是**红黑树**（Red-Black Tree），一种自平衡二叉搜索树。
    - 元素按**键的顺序**存储，因此 `map` 是有序的。
        
- **`unordered_map`**：
    - 底层实现是**哈希表**（Hash Table）。
    - 元素按**哈希值**存储，因此 `unordered_map` 是无序的。
        
***2. 性能特性***
 **插入（Insert）**
- **`map`**：
    - **时间复杂度**：平均情况下为 O(log n)，因为红黑树的插入操作需要维护平衡。
    - **空间复杂度**：O(n)。
        
- **`unordered_map`**：
    - **时间复杂度**：平均情况下为 O(1)，因为哈希表的插入操作通常是常数时间。
    - **空间复杂度**：O(n)。
        
**查找（Find）**
- **`map`**：
    - **时间复杂度**：平均情况下为 O(log n)，因为红黑树的查找操作需要遍历树的高度。
        
- **`unordered_map`**：
    - **时间复杂度**：平均情况下为 O(1)，因为哈希表的查找操作通常是常数时间。
		
**删除（Erase）**
- **`map`**
    - **时间复杂度**：平均情况下为 O(log n)，因为红黑树的删除操作需要维护平衡。
- **`unordered_map`**：
    - **时间复杂度**：平均情况下为 O(1)，因为哈希表的删除操作通常是常数时间。
        
***3. 适用场景***
- **`map`**：
    - 适用于需要**有序**存储键值对的场景。
    - 适用于需要频繁进行**范围查询**（如 `lower_bound`、`upper_bound`）的场景。
    - 适用于需要**稳定**性能的场景，因为红黑树的性能不会因为数据分布不均而显著下降。
        
- **`unordered_map`**：
    - 适用于需要**快速查找**、**插入**和**删除**的场景。
    - 适用于不需要有序存储键值对的场景。
    - 适用于数据分布均匀的场景，因为哈希表的性能依赖于哈希函数的质量。
        
***4. 其他***
- **内存使用**：
    - `unordered_map` 通常比 `map` 占用更多的内存，因为哈希表需要额外的空间来处理冲突。
        
- **哈希函数**：
    - `unordered_map` 的性能依赖于哈希函数的选择。如果哈希函数不好，可能会导致性能下降。
        
- **迭代器失效**：
    - `map` 的迭代器在插入和删除操作后仍然有效（除非删除的是当前迭代器指向的元素）。
    - `unordered_map` 的迭代器在插入操作后可能失效，因为哈希表可能会重新分配内存。


#### ***位运算*** 
我们考虑到**异或**具有这样**的性质**：
1. 0 异或 任何数，皆等于 任何数
2. 两个相同的数 异或，都等于 0

而我们的数列是由$n/2$对相同的数字和$1$个单独的数组成，所以答案就是*这组数的异或*。
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int x,ans = 0;
    int n;cin >> n;
    for(int i = 1;i <= n;i ++)
        cin >> x,ans ^= x;
    cout << ans << '\n';
    return 0;
}
```

## B 魔法符文的有效性
### 题意：
给你一组只由`()[]{}` 组成的字符串，你需要验证它们括号是否匹配，输出`Yes`或`No`。
### 题解：
括号匹配问题 => 联想到*栈stack*
```cpp
#include <stdc++.h>
using ll = long long;

bool isValid(const std::string& s) {
    std::stack<char> st;
    for (char ch : s) {
        if (ch == '(' || ch == '[' || ch == '{') {
            st.push(ch);
        } else {
            if (st.empty()) {
                return false;
            }
            char top = st.top();
            if ((ch == ')' && top == '(') ||
                (ch == ']' && top == '[') ||
                (ch == '}' && top == '{')) {
                st.pop();
            } else {
                return false;
            }
        }
    }
    return st.empty();
}

int main() {
	ios_sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int T;
    std::cin >> T;

    while(T --) {
        std::string s;
        cin >> s;
        if (isValid(s)) {
            std::cout << "Yes" << '\n';
        } else {
            std::cout << "No" << '\n';
        }
    }

    return 0;
}
```

## C 科研项目攻关
### 题意：
拓补排序模板题，给$T$组数据，每组第一行输入$N,M$分别代表节点数和点之间的关系，接下来$M$行每行输入$<a,b>$表示在启动$a$之前必须先启动$b$。问你给出的图能否进行拓补排序。
### 题解：
拓补排序解题思路：每次寻找入度为 $0$ 的点，将其入对，再将该队首的点的所有邻接点入度$-1$，如出现入度为 $0$ 的点再入队，知道队列为空结束。
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int N = 2e5 + 9;

vector<int> g[N]; // 邻接表
int inDegree[N]; // 入度

void solve()
{
	// 输入
	int n,m,a,b;cin >> n >> m;
	// 初始化/归零
	for(int i = 1;i <= n;i ++) g[i].clear(),inDegree[i] = 0;
	
	for(int i = 0;i < n;i ++)
	{
		cin >> a >> b; // 输入
		g[b].push_back(a); // 构建邻接表
		inDegree[a] ++; // 计算a结点的入度
	}
	
	queue<int> q;
	for(int i = 1;i <= n;i ++)
		if(inDegree[i] == 0) q.push(i);
	
	// 处理队列中的节点数
    int processedCount = 0;
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        processedCount++;
        // 将所有邻接节点的入度减 1
        for (int v : g[u]) {
            inDegree[v]--;
            if (inDegree[v] == 0) {
                q.push(v);
            }
        }
    }

    // 判断是否所有节点都被处理
    cout << (processedCount == n ? "Yes" : "No") << '\n';
    return ;
}

int main() {
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int T;cin >> T;
	while(T --)
	{
		solve();
	}
	return 0;
}
```

