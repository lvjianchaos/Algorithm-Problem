$time\ limit\ per\ test:\ 1\ second$
$memory\ limit\ per\ test:\ 1024\ megabytes$
共有 $n$ 个宝石，第 $i$ 颗宝石的价格为 $w_i$ 美元，美丽度为 $v_i$ ，你有 $W$ 美元。

今天搞促销，顾客可以选择任意 $k$ 颗宝石免费。询问：若你采取最佳策略，可以用 $W$ 美元获得最大的总美丽度是多少？

$1 \le n \le 5 \times 10^3$, $1 \leq W \leq 10^4$, $0 \leq k \leq n$
$1 \leq w_i \leq W$, $1 \leq v_i \leq 10^9$

###### 【01 背包】【优先队列】
观察得：
1. 如果我们免费选择 $a$ ，花钱选择 $b$ ，那么 $W_a\ge W_b$；否则，就会造成损失。
2. 如果我们免费选择 $a$ ，不选择 $b$ ，那么 $V_a \ge V_b$；否则，就会造成损失。

