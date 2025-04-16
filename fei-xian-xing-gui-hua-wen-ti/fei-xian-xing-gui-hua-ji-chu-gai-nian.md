# 非线性规划基础概念

### 什么是非线性规划?

当数学规划模型中的目标函数或者约束条件不是线性表达式时, 该模型被称作**非线性规划 (Nonlinear Programming)** 模型

一般我们会把非线性规划问题分成无约束问题和约束问题两类, 无约束问题的表达式为:

$$
\begin{aligned}
min \quad f(x) \\
s.t \quad x \in \Omega
\end{aligned}
$$

其中, $$\Omega$$ 代表可行域, 没有额外说明的话我们一般假设 $$\Omega = E^{n}$$

约束问题的表达式为:

$$
\begin{aligned}
min \quad & f(x) \\
s.t \quad & g_{i}(x) \geq 0, i \in \{1,...,p\}\\
\quad & h_{j}(x) = 0, j \in \{1,...,m\}\\
\quad & x \in \Omega
\end{aligned}
$$

同样 $$\Omega$$ 代表可行域, 没有额外说明的话我们一般假设 $$\Omega = E^{n}$$

**定义 局部最小点 (Local Minimum Point)** 如果对于点 $x^{_} \in \Omega$, 存在 $\epsilon > 0$ 使得所有满足 ${x\in \Omega||x- x^{_}| < \epsilon }$ 的点有 $f(x) \geq f(x^{_})$, 则我们称点 $x^{_}$ 为局部最小点, $f(x^{\*})$ 为局部最小值

**定义 全局最小点 (Global Minimum Point)** 如果对于点 $x^{_} \in \Omega$, 对于所有点 $x\in \Omega$ 有 $f(x) \geq f(x^{_})$, 则我们称点 $x^{_}$ 为全局最小点, $f(x^{_})$ 为全局最小值

**定义 局部最大点 (Local Maximum Point)** 如果对于点 $x^{_} \in \Omega$, 存在 $\epsilon > 0$ 使得所有满足 ${x\in \Omega||x- x^{_}| < \epsilon }$ 的点有 $f(x) \leq f(x^{_})$, 则我们称点 $x^{_}$ 为局部最大点, $f(x^{\*})$ 为局部最大值

**定义 全局最大点 (Global Maximum Point)** 如果对于点 $x^{_} \in \Omega$, 对于所有点 $x\in \Omega$ 有 $f(x) \leq f(x^{_})$, 则我们称点 $x^{_}$ 为全局最大点, $f(x^{_})$ 为全局最大值
