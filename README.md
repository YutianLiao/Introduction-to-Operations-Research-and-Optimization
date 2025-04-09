与线性规划不同, <font color = #808080>__整数规划 (Integer Programmin)__</font> 问题的解必须是整数. 比如当变量是人数时候, 分数解显然是不合理的. 当所有变量必须为整数时, 我们称该问题为纯整数规划. 当变量一部分必须为整数, 而另一部分可以为分数时候, 我们称该问题为 <font color = #808080>__混合整数规划 (Mixed Integer Programming)__</font>

$$
\begin{aligned}
max \quad & c^{T}x \\
s.t \quad & Ax = b\\
\quad & x \in \mathbb{Z}
\end{aligned}
$$

在2.1中的例子中最优解为 (0.5, 8), $x_{1}$ 不是整数, 所以不符合约束条件. 该例子的最优整数解为 (0, 8), 对应的目标函数是16

<font color = #32CD32>__定理__</font> 对于同一个约束条件 $Ax = b$ 和同一个目标函数 $\max c^{T}x$. 设 $OPT_{LP}$ 为线性规划的最优解, $OPT_{IP}$ 为整数规划的最优解, 则 $OPT_{IP} \leq OPT_{LP}$
