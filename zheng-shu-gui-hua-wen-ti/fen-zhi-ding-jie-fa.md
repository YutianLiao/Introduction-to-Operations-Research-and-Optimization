# 分支定界法

{% hint style="info" %}
**分支定界法 (Branch and Bound)** 是解决整数规划问题 (纯整数规划问题和混合整数规划问题) 的重要方法, 由Ailsa Land和Alison Doig在1960年提出. 它是一种通过“分支”和“定界”两个步骤来有效搜索解空间的方法
{% endhint %}

### 核心思想

* **分支 (Branching):** 指将原问题分解为多个子问题. 每个子问题的解空间比原问题小, 从而可以通过递归或迭代的方式, 逐步探索到所有可能的解. 这个分解过程通常是二叉树形式 (也可以是其他形式), 每个子问题表示为树的一个节点. 分支的目的是通过探索更小的解空间, 逐步逼近最优解
* **定界 (Bounding):** 指在探索子问题时, 计算当前子问题的上界或下界. 上界和下界反映了当前子问题的最优解的可能范围
* **剪枝 (Pruning):** 指通过定界步骤, 去掉那些不可能得到更优解的节点. 对于某些子问题, 如果其上界已经小于当前的最优解，就可以直接排除这个子问题的继续计算 (即“剪枝”). 通过这种方式，分支定界法可以在搜索过程中大大减少计算量

***

### **算法过程**

1. (初始化) $$D = {N_{0}}$$, $$\underline{z} = -\infty$$, $$(x, y) := None$$
2. (停止条件) 当 $$D$$ 为空集时, 返回 $$(x,y)$$
3. (选择一个节点) 从 $$D$$ 中选择一个节点 $$N_{i}$$, 然后将其从 $$D$$ 上删除
4. (定界) 求解 $$N_{i}$$ 对应的线性规划问题, 如果问题没有解, 则返回第二步; 如果有解, 则记 $$(x_{i}, y_{i})$$ 是该线性规划问题的最优解, $$z_{i}$$ 是对应的目标函数值
5. (剪枝) 如果 $$z_{i} \leq \underline{z}$$, 则返回第二步; 否则, 如果 $$(x_{i}, y_{i})$$ 是原整数 (混合整数) 规划问题的解, 则让 $$\underline{z} = z_{i}$$, $$(x, y) = (x_{i}, y_{i})$$; 如果该解不符合要求, 则进行步骤6
6. (分支) 将 $$N_{i}$$ 对应的线性规划问题分成若干个子问题 (包含该线性规划问题所有整数解 (混合整数解)), 将这些节点加入 $$D$$, 然后返回步骤2

***

### 代码实现

```python
import gurobipy as gp
import numpy as np
from gurobipy import GRB

def branch_and_bound(c, A, b):
    D = [(c, A, b)]; lower_bound = -np.inf; optimal_point = None; _, n = A.shape; k = 0

    while D:
        k += 1; c, A, b = D.pop(0)

        model = gp.Model(f"linear_programming_example{k}"); model.setParam('OutputFlag', 0)
        x = model.addVars(len(c), name="x"); model.setObjective(gp.quicksum(c[i] * x[i] for i in range(len(c))), gp.GRB.MAXIMIZE)
        for i in range(len(b)):
            model.addConstr(gp.quicksum(A[i, j] * x[j] for j in range(len(c))) <= b[i], name=f"constraint_{i+1}")
        model.optimize()

        if model.status != GRB.OPTIMAL: continue
        else:
            point = [round(x[i].X,2) for i in range(len(c))]; objective_value = model.objVal
            if np.all(np.array(point) % 1 == 0): lower_bound = objective_value; optimal_point = point
            else:
                if objective_value <= lower_bound: continue
                else:
                    index = np.argmax(np.array(point) % 1)
                    new_constraint = np.zeros(n); new_constraint[index] = 1
                    point1 = (c, np.vstack([A, new_constraint]), np.hstack([b, np.floor(point[index])])); point2 = (c, np.vstack([A, -new_constraint]), np.hstack([b, -np.ceil(point[index])]))
                    D.append(point1); D.append(point2)
        
    return optimal_point, lower_bound

if __name__ == "__main__":
    c = np.array([3, 2]); A = np.array([[4, 1], [2, 3], [2, 1]]); b = np.array([10, 25, 20])
    print(branch_and_bound(c, A, b))
```

```
([0.0, 8.0], 16.0)
```
