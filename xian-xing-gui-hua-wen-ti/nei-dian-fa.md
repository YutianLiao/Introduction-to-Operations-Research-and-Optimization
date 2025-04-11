# 内点法

{% hint style="info" %}
**内点法**是求解线性规划问题 (以及其他优化问题) 的一种重要算法, 最初由Karmarkar在1984年提出. 它是线性规划中的一种重要解法, 与传统的单纯形法相比, 内点法在大规模问题的求解中展示出了巨大的优势
{% endhint %}

### 核心思想

通过沿着可行域的内部而非边界来搜索最优解: 传统的单纯形法通过在约束的边界上移动来寻找最优解, 而内点法则通过从可行区域的内部出发, 逐步逼近最优解的边界, 最终到达最优顶点

***

### 放射尺度算法

算法的核心思想是: 虽然直接在多面体 $$P$$ 上最大化 $$c^{T}x$$ 可能比较困难, 但在椭圆体上优化相同的目标函数却比较容易. 因此, 我们不直接在 $$P$$ 上最大化, 而是求解一系列在椭圆体上的优化问题. 首先从一个可行解 $$x_{0} > 0$$ 开始, 位于 $$P$$ 的内部, 我们构造一个以 $$x_{0}$$ 为中心的椭圆体 $$S_{0}$$, 它包含于 $$P$$ 的内部. 然后, 我们在椭圆体 $$S_{0}$$​ 内优化线性目标函数 $$c^{T}x$$, 找到一个新的内部点 $$x_{1}$$. 我们画出另一个以 $$x_{1}$$ 为中心的椭圆体 $$S_{1}$$, 并以类似的方式继续进行, 如上图所示

```python
import numpy as np

def affine_scalling_algorithm(c, A, b, episilon = 1e-6, beta = 0.1):
    A = np.array(A, dtype=float); b = np.array(b, dtype=float); c = np.array(c, dtype=float)
    m, n = A.shape; A = np.hstack([A, np.eye(m)]); c = np.hstack([c, np.zeros(m)])
    x_k = []; new_b = b.copy()
    for i in range(n): x_k.append(min(1, np.min(list(map(lambda a, b: a / b, new_b, A.T[i]))))); new_b = new_b - x_k[i] * A.T[i]
    x_k = np.hstack([x_k, new_b])

    while True:
        X_k = np.diag(x_k)
        p_k = -np.linalg.inv(A @ X_k @ X_k @ A.T) @ A @ X_k @ X_k @ c
        r_k = -c - A.T @ p_k

        if np.all(r_k >= 0) and np.all(np.ones(m+n).T @ X_k @ r_k < episilon) : return list(map(lambda num: round(num, 1), x_k[:n]))
        elif np.all(X_k @ X_k @ r_k <= 0): return "Problem is unbounded"
        else: x_k = x_k - beta * ( X_k @ X_k @ r_k) / np.linalg.norm(X_k @ r_k)

c = np.array([3, 2])
A = np.array([[4, 1], [2, 3], [2, 1]])
b = np.array([10, 25, 20])
print(affine_scalling_algorithm(c, A, b))
```

***

### Potential Reduction算法

算法的核心思想是: 在仿射尺度算法中, 迭代序列会迅速接近可行集的边界, 而算法被迫在逼近椭圆体变得越来越小的过程中采取非常小的步伐. 如果让当前点远离可行集的边界, 这样算法就能够在后续的步骤中取得更大的进展. 直观地说, Potential Reduction算法采用一种内部点算法, 在朝着最优性方向进展的同时, 能够通过减少目标函数值并远离可行集的边界来实现

为了实现这个目标, 我们定义 $$G(x,s) = q \log s^{T}x - \sum_{j=1}^{n}\log x_{j} - \sum_{j=1}^{n}\log s_{j}$$ ($$q$$ 是一个大于 $$n$$ 的常数). 第一项是度量原问题和对偶问题的差距, 第二和第三项分别惩罚原始解和对偶解接近可行集边界的程度

```
// Some code
```

