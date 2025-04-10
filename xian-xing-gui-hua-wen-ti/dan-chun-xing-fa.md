# 单纯形法

{% hint style="info" %}
单纯形法自从 1947 年由 Georges Dantzig 提出以来, 一直是求解线性规划问题的经典方法. 尽管它在理论上在最坏情况下有[指数时间复杂度](https://sophie.huiberts.me/files/Klee-Minty-How-Good-Is-the-Simplex-Algorithm-1970.pdf), 但从实际应用的角度来看, 单纯形法仍然是一种非常高效的算法
{% endhint %}

### 核心思想

先找出一个初始的基可行解 (顶点), 通过沿着约束边界移动到相邻顶点, 逐步提升目标函数值, 直到找到最优解



### 代码实现

```python
import numpy as np

'''
解决的问题为 max c^T x
    subject to Ax <= b
'''

def simplex(c, A, b):
    A = np.array(A, dtype=float)
    b = np.array(b, dtype=float)
    c = np.array(c, dtype=float)
    
    # 将问题转化为标准型, 即 (Ax <= b) -> (Ax + y = b)
    m, n = A.shape
    A = np.hstack([A, np.eye(m)])
    c = np.hstack([c, np.zeros(m)])
    num_vars = n + m
    
    
    basic_vars = list(range(n, num_vars))
    non_basic_vars = list(range(n))
    
    while True:
        cb = c[basic_vars]
        x_b = np.linalg.solve(A[:, basic_vars], b)
        z = cb @ x_b
        reduced_costs = c[non_basic_vars] - cb @ np.linalg.inv(A[:, basic_vars]) @ A[:, non_basic_vars]
        if np.all(reduced_costs <= 0): break
        entering_var = non_basic_vars[np.argmax(reduced_costs)]
        ratios = np.full(m, np.inf)
        for i in range(m):
            if A[i, entering_var] > 0: ratios[i] = b[i] / A[i, entering_var]
        leaving_var_index = np.argmin(ratios)
        leaving_var = basic_vars[leaving_var_index]
        basic_vars[leaving_var_index] = entering_var; non_basic_vars.remove(entering_var); non_basic_vars.append(leaving_var)
        pivot_val = A[leaving_var_index, entering_var]; A[leaving_var_index, :] /= pivot_val; b[leaving_var_index] /= pivot_val
        for i in range(m):
            if i != leaving_var_index: 
                factor = A[i, entering_var]; A[i, :] -= factor * A[leaving_var_index, :]; b[i] -= factor * b[leaving_var_index]
    optimal_solution = z; solution = np.zeros(num_vars)
    for i in range(m):
        if basic_vars[i] < n: solution[basic_vars[i]] = x_b[i]
    return optimal_solution, solution[:n]

# 测试算法
c = np.array([3, 2])
A = np.array([[4, 1], [2, 3], [2, 1]])
b = np.array([10, 25, 20])
optimal_solution, solution = simplex(c, A, b)
print(f"Optimal Solution: {optimal_solution}\nSolution: {solution}")
```

```
Optimal Solution: 17.5
Solution: [0.5 8. ]
```
