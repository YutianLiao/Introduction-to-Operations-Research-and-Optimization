# 单纯形法

{% hint style="info" %}
**单纯形法**自从 1947 年由 Georges Dantzig 提出以来, 一直是求解线性规划问题的经典方法. 尽管它在理论上在最坏情况下有[指数时间复杂度](https://sophie.huiberts.me/files/Klee-Minty-How-Good-Is-the-Simplex-Algorithm-1970.pdf), 但从实际应用的角度来看, 单纯形法仍然是一种非常高效的算法
{% endhint %}

### 核心思想

{% hint style="info" %}
<mark style="color:red;">**定理**</mark> 线性规划问题的顶点就是一个基可行解
{% endhint %}

{% hint style="info" %}
<mark style="color:red;">**定理**</mark> 如果线性规划问题的可行域有界, 则其最优解必可在某个顶点处获得
{% endhint %}

由上述定理得到启发, 我们可以先找出一个初始的基可行解 (顶点), 然后通过沿着约束边界移动到相邻顶点, 逐步提升目标函数值, 直到找到最优解

***

### 算法第一步: 确定初始基可行解

假设A中已经存在一个单位对角矩阵 ($$I_{m}$$): (不失一般性)

$$
A = \begin{bmatrix}
1 & 0 & \cdots & 0 & a_{1(m+1)} & \cdots & a_{1n} \\
0 & 1 & \cdots & 0 & a_{2(m+1)} & \cdots & a_{2n} \\
\cdots & \cdots & \ddots & \cdots & \cdots & \ddots & \cdots\\
0 & 0 & \cdots & 1 & a_{m(m+1)} & \cdots & a_{mn} \\
\end{bmatrix}
$$

将所有基变量用非基变量的函数式来表示, 可得 $$x_{i} = b_{i} - \sum_{j=m+1}^{n}a_{ij}x_{j}, j =1,2,...,m$$. 相应的, 也将目标函数用非基变量的函数式表达为: $$z = \sum_{i=1}^{m}c_{i}b_{i} + \sum_{j=m+1}^{n}(c_{j}-\sum_{i=1}^{m}a_{ij}c_{i})x_{j}$$. 此时, 如果令所有非基变量等于零, 可以得到初始基可行解为 $$X^{(0)} = (x_{1},x_{2},...,x_{n})^{T}=(b_{1},b_{2},...,b_{m},0,...,0)^{T}$$, 它对应的目标函数值为 $$z^{(0)}=\sum_{i=1}^{m}c_{i}b_{i}$$

如果不存在一个单位对角矩阵, 我们可以通过两个方法来得到一个单位对角矩阵:

> <mark style="color:blue;">**大M法 (Big-M Method)**</mark>

考虑以下例子: $$max$$ $$z = 3x_{1} + 2x_{2} - 7x_{3}$$

$$
A = \begin{bmatrix}
1 & 2 & 3 \\
0 & 3 & 6\\
0 & 5 & 8 \\
\end{bmatrix}
$$

我们引入两个新的人工变量 $$x_{4}$$, $$x_{5}$$, 这时 $$A$$ 就含有了一个单位对角矩阵:

$$
A = \begin{bmatrix}
1 & 2 & 3 & 0 & 0 \\
0 & 3 & 6 & 1 & 0\\
0 & 5 & 8 & 0 & 1\\
\end{bmatrix}
$$

但是容易观察到, 两个问题的可行域是不同的, 我们需要保证 $$x_{4}$$, $$x_{5}$$ 最后的取值为0. 我们通过调整目标函数来实现这一点: $$max$$ $$z = 3x_{1} + 2x_{2} - 7x_{3} - Mx_{4} - Mx_{5}$$ ($$M$$ 是一个无穷大的正数). 如果原始问题的可行域非空, 那么优化上述新问题得到的最优解一定满足 $$x_{4}=x_{5}=0$$.

> <mark style="color:blue;">**两阶段法 (Two-Phase Method)**</mark>

第一阶段: 构造一个辅助的线性规划模型, 其目标函数是最小化人工变量之和, 约束条件是引入人工变量后的等式形式. 利用单纯形法求解该辅助问题, 当某时候所有人工变量取值为零时, 则第一阶段的最优解便是原问题的一个基可行解. 进入第二阶段

第二阶段: 删除人工变量, 将目标函数系数替换为原始问题的目标函数系数, 利用单纯形法求解

***

### 算法第二步: 判断解的最优性

> $$\lambda_{j}$$ 的经济含义是: 机会成本, 即在保持问题可行性前提下增加变量 $$x_{j}$$ 能获得的对目标函数的改进

记 $$\lambda_{j}:=c_{j}-c_{B}^{T}B^{-1}A_{j}$$, 其中 $$c_{B}$$ 为当前基变量的成本系数向量, $$B$$ 为基变量对应的系数矩阵, $$A_{j}$$ 为第 $$j$$ 个非基变量的系数列. 如果所有检验系数 $$\lambda_{k}\leq 0$$, 则当前 $$x^{*}$$ 为线性规划问题的最优解

***

### 算法第三步: 换基迭代

记 $$x_{j}$$ 为已经确定的入基变量, 为了确定出基变量, 我们将其他变量取值为 $$0$$ 不变, 来确定 $$x_{j}$$ 的最大取值. 定义 $$\theta_{i}$$ = $$\frac{(B^{-1}b)_{i}}{(B^{-1}A_{j})_{i}}$$, $$(B^{-1}A_{j})_{i} > 0$$

{% hint style="info" %}
<mark style="color:red;">**定理**</mark> 若所有 $$(B^{-1}A_{j})_{i} \leq 0$$, 那么该线性规划问题无有界最优解
{% endhint %}

以下我们考虑 $$(B^{-1}A_{j})_{i}$$ 中至少有一个为正的情形. 则至少某一基变量 (记为 $$x_{r}$$) 符合 $$b_{r}-a_{rk}\theta =0$$, 选择 $$x_{r}$$ 为出基变量

> <mark style="color:blue;">**Bland’s anticycling pivoting rule**</mark>

对于入基变量, 在所有 $$\lambda_{j} > 0$$ 中, 选择下标最小的. 对于出基变量, 选择比率最小的为出基变量, 如果出现多个候选的情况，选择下标最小的一个

{% hint style="info" %}
<mark style="color:red;">**定理**</mark> 基于Bland’s anticycling pivoting rule的单纯形法最终会停止 (即找到最优解)
{% endhint %}

***

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
        # 计算减少成本
        cb = c[basic_vars]
        x_b = np.linalg.solve(A[:, basic_vars], b)
        z = cb @ x_b
        reduced_costs = c[non_basic_vars] - cb @ np.linalg.inv(A[:, basic_vars]) @ A[:, non_basic_vars]
        
        # 判断解的最优性
        if np.all(reduced_costs <= 0): break
        
        entering_var = non_basic_vars[np.argmax(reduced_costs)]
        ratios = np.full(m, np.inf)
        for i in range(m):
            if A[i, entering_var] > 0: ratios[i] = b[i] / A[i, entering_var]
        leaving_var_index = np.argmin(ratios)
        leaving_var = basic_vars[leaving_var_index]
        basic_vars[leaving_var_index] = entering_var
        non_basic_vars.remove(entering_var); non_basic_vars.append(leaving_var)
        pivot_val = A[leaving_var_index, entering_var]
        A[leaving_var_index, :] /= pivot_val
        b[leaving_var_index] /= pivot_val
        for i in range(m):
            if i != leaving_var_index: 
                factor = A[i, entering_var]
                A[i, :] -= factor * A[leaving_var_index, :]
                b[i] -= factor * b[leaving_var_index]
    optimal_solution = z
    solution = np.zeros(num_vars)
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
