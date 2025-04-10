# 线性化方法

### 乘积式

多个0-1变量相乘 $$x_{1}x_{2}..x_{n}$$ 等同于

$$
\begin{aligned}
& y \leq x_{i} & \forall i = 1,...,n \\
& y \geq \sum_{i=1}^{n}x_{i} - (n - 1)\\
& x_{i},y \in\{0,1\} & \forall i = 1,...,n
\end{aligned}
$$

若 $$x_{1} \in {0,1}$$, $$x_{2} \in [0,u]$$. 则 $$x_{1}x_{2}$$ 等同于

$$
\begin{aligned}
& y \leq ux_{1} \\
& y \leq x_{2}\\
& y \geq x_{2} - u(1-x_{1})\\
& x_{1} \in \{0,1\}, x_{2} \in [0,u], y \in [0,u]
\end{aligned}
$$

若 $$x_{1} \in {0,1}$$, $$x_{2} \in [l,u]$$. 则 $$x_{1}x_{2}$$ 等同于

$$
\begin{aligned}
& y \leq x_{2}\\
& y \geq x_{2} - u(1-x_{1})\\
& lx_{1} \leq y \leq ux_{1}\\
& x_{1} \in \{0,1\}, x_{2} \in [l,u], y \in [l,u]
\end{aligned}
$$

对于两个连续变量相乘的情况是无法进行等价线性化的, 只能进行近似. 近似的常用方法是 [McCormick Envelope](https://optimization.cbe.cornell.edu/index.php?title=McCormick_envelopes)

### 取整

等式 $$\lceil \frac{x}{Q} \rceil$$ ($$x$$ 为非负连续变量, $$y$$ 是非负整数变量, $$Q$$ 为任意整数) 等同于

$$
\begin{aligned}
min \quad & y \\
s.t \quad & y \geq \frac{x}{Q}\\
\quad & y - 1 \leq \frac{x}{Q}
\end{aligned}
$$

### 绝对值

对于约束 $$z = |x|$$, 其中 $$x$$ 为任意类型的决策变量, $$z$$ 为非负决策变量. 假设 $$|x| \leq M$$, 我们引入辅助决策变量 $$x_{p} \geq 0$$, $$x_{n} \geq 0$$, $$y \in {0,1}$$, $$x_{p}$$ 和 $$x_{n}$$ 分别表示 $$x$$ 为正数和负数时的绝对值, $$y$$ 表示 $$x$$ 的正负性 (若 $$x\geq 0$$, 则 $$y=1$$; 否则 $$y=0$$). 这时原约束等同于

$$
\begin{aligned}
& z = x_{p} + x_{n}\\
& x = x_{p} - x_{n}\\
& x_{p} \leq My\\
& x_{n} \leq M(1-y)\\
& y\in\{0,1\}, z,x_{p},x_{n} \geq 0
\end{aligned}
$$

### $$min/max$$ 函数

考虑约束 $$y = max\{x_{1}, x_{2}\}$$, 其中 $$y, x_{1}, x_{2}$$ 均为决策变量. 该约束等同于

$$
\begin{aligned}
min \quad & y \\
s.t \quad & y \geq x_{1}\\
\quad & y \geq x_{2}
\end{aligned}
$$

对于约束 $$y = min\{x_{1}, x_{2}\}$$, 其中 $$y, x_{1}, x_{2}$$ 均为决策变量. 该约束等同于

$$
\begin{aligned}
max \quad & y \\
s.t \quad & y \leq x_{1}\\
\quad & y \leq x_{2}
\end{aligned}
$$
