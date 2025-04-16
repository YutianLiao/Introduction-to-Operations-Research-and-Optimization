# 图的遍历

{% hint style="info" %}
**图的遍历 (Traversing Graph Structure)** 指从已给的连通图中某一顶点出发, 沿着一边访遍图中所有的顶点, 且使每个顶点仅被访问一次
{% endhint %}

### 深度优先搜索 (DFS)

核心思想是: 从图的一个起始节点开始, 沿着每一条边尽可能深的遍历图, 直到访问到一个没有未访问相邻节点的节点为止, 然后回溯到上一个节点, 继续遍历

```python
def dfs(graph, node, visited=None):
    if visited is None:
        visited = set()  # 记录已访问的节点
    visited.add(node)  # 标记当前节点为已访问
    
    print(node, end=' ')  # 处理当前节点，输出当前节点
    
    for neighbor in graph[node]:
        if neighbor not in visited:  # 如果邻居未访问过
            dfs(graph, neighbor, visited)  # 递归访问邻居
    
    return visited
```

***

### 广度优先搜索 (BFS)

核心思想是: 从图的一个起始节点开始, 首先访问起始节点的所有邻居, 然后逐层扩展, 访问未访问的邻居

```python
from collections import deque

def bfs(graph, start):
    visited = set()  # 记录已访问的节点
    queue = deque([start])  # 使用队列来存储待访问的节点
    visited.add(start)
    
    while queue:
        node = queue.popleft()  # 取出队列中的第一个节点
        print(node, end=' ')  # 处理当前节点，输出当前节点
        
        for neighbor in graph[node]:
            if neighbor not in visited:  # 如果邻居未访问过
                visited.add(neighbor)  # 标记邻居为已访问
                queue.append(neighbor)  # 将邻居添加到队列中
```
