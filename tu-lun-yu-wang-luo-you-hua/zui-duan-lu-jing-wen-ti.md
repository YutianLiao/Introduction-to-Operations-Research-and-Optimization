# 最短路径问题

{% hint style="info" %}
**最短路径 (Shortest Path)** 是图中从一个顶点到另一个顶点的路径，且该路径的边权重和最小; 最短路径问题是重要的优化问题之一, 被应用于多个领域, 如: 路线规划, 物流与配送, 厂区布局等
{% endhint %}

### Dijkstra算法

给定源头 $$s \in V$$

1. (初始化)
   1. $$S:= \empty$$, $$\overline{S}:= V$$
   2. $$d[i] := \infty$$ ($$\forall i \in V$$)
   3. $$pred[i] := \empty$$ ($$\forall i \in V$$)
2. (初始化源头信息) $$d[s] := 0$$, $$pred[s] := s$$
3. (算法停止条件) 如果 $$|S| = |V|$$, 算法停止, 返回列表 $$d$$ ($$d[i]$$ 代表 $$s$$ 到 $$i$$ 的最短路径) 和列表 $$pred$$; 如果 $$|S| < |V|$$, 进行步骤4
4. (算法主体)
   1. 记 $$i = \argmin_{j} {d[j] (j \in \overline{S}})$$
   2. 让 $$S = S \cup {i}$$, $$\overline{S} = \overline{S} - {i}$$$$S = S \cup {i}$, $\overline{S} = \overline{S} - {i}$$
   3. 对于所有 $$e= (i,j ) \in E$$, 如果 $$d[j] > d[i] + c_{ij}$$, 则记 $$d[j] = d[i] + c_{ij}$$, $$pred[j] = i$$
   4. 返回步骤3

{% hint style="info" %}
<mark style="color:red;">**注释**</mark> $$pred[i]$$ 记录的是源头 $$s$$ 到 $$i$$ 的最短路径的上一个顶点, 如果需要找到从 $$s$$ 到 $$t$$ 的最短路径经过的所有顶点, 则不断访问 $$pred$$ 即可
{% endhint %}

***

### Bellman-Ford算法

给定源头 $$s \in V$$

1. (初始化)
   1. $$d[i] := \infty$$ ($$\forall i \in V$$)
   2. $$pred[i] := \empty$$ ($$\forall i \in V$$)
   3. $$d[s] := 0$$, $$pred[s] := s$$
2. (松弛操作, 重复 $$|V| -1$$ 次) 对于所有的边 $$e = (v_{i}, v_{j})\in E$$, 如果 $$d[i] < \infty$$ 并且 $$d[j] > d[i] + c_{ij}$$: 则记 $$d[j] = d[i] + c_{ij}$$, $$pred[j] = i$$
3. (检查负数环存在) 再进行一次松弛操作, 如果依然存在边使得列表 $$d$$ 更新, 则返回 “存在负数环”; 反之, 返回列表 $$d$$ 和列表 $$pred$$

***

### A\*搜索算法

A\* 算法是一种启发式搜索算法, 它结合了贪心算法和Dijkstra算法, 广泛应用于图形搜索和路径规划. A\* 算法不仅考虑从起点到当前节点的最短路径 (如Dijkstra算法), 还考虑从当前节点到目标节点的估计代价 (启发式估计)

{% hint style="info" %}
<mark style="color:red;">**定义 启发式函数 (Heuristic Function)**</mark> 让 $$g(i)$$ 表示从起点 $$s$$ 到当前节点 $$i$$ 的实际距离, $$h(i)$$ 表示从 $$i$$ 到目标节点 $$t$$ 的估计距离; 则 $$f(i) := g(i) + h(i)$$ 表示的就是从起点到终点且经过点 $$i$$ 的估计距离
{% endhint %}

A\*搜索算法唯一与Dijkstra算法不同的是, Dijkstra算法每次选取的是当前距离 $$g(i)$$ 最短的点, 而A\*搜索算法选取的是 $$f(i)$$ 最短的点

{% hint style="info" %}
<mark style="color:red;">**注释**</mark> 当 $$h(i) = 0$$ 时候, A\*搜索算法等同于Dijkstra算法; 在平面地图上搜索路径时, $$h(i)$$ 一般表示直线距离: $$h(i) = \sqrt{(i_{1}-t_{1})^{2} + (i_{2}-t_{2})^{2}}$$
{% endhint %}

