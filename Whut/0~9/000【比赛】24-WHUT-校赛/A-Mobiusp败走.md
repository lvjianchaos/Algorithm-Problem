**比赛场次**

**比赛题目**

<!--more-->

3 seconds / 1024 megabytes

standard input / standard output

## 题目

Mobiusp 给 Loop 设计了一款游戏，游戏规则如下:

 给定一纵轴有 n 个方块，横轴有 m 个方块的图，其中：

 "#" 代表不能移动的方块

 "." 代表空白的方块

 "*" 代表可以移动的方块

 图的原点位于左上角,左上角第一个方块的坐标为 (1,1)，右下角最后一个方块的坐标为：(n,m)

 Mobiusp 给出了一个特殊的 "*"，将其标记为 "1"，并给出了一个坐标，让 Loop 判断 "1" 能不能通过非负整数次**移动**到达这个坐标。

 **移动**的规则如下：

 一次移动可以将空白方块与上下左右可移动的方块进行交换，如：

```
 #***
 
 **.*
 
 **#*
```

 

 **可以**经过一次移动变成

```
#*.*

****

**#*
```

 

 但**不能**交换 '#' 变成

```
 #***

 **#*

 **.*
```



## 输入

第一行两个整数 n, m 代表图的大小 (2≤n,m≤1500)。

第二行两个整数 x, y 代表给定的坐标 (1≤x≤n, 1≤y≤m)。

接下来 n 行，每行 m 个字符代表给定的图。

## 输出

如果能请输出 Yes，不能请输出 No。

## 样例

**输入**

> 3 4
> 2 1
> `#***`
> `**.1`
> `**#*`

**输出**

> No

**输入**

> 3 4
> 2 2
> `#***`
> `**.1`
> `**#*`

**输出**

> Yes

## 解释

题目保证图中有且只有一个 '.' ，有且只有一个 '1'。

## 题解

<!--文本-->

## 代码

<!--代码-->