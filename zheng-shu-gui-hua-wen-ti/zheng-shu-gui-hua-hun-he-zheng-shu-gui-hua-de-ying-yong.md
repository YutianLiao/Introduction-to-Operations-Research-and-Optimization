# 整数规划/混合整数规划的应用

### **设施选址问题 (Facility Location Problem)**

通常如下提出: 假设有 $$n$$ 设施和 $$m$$ 客户. 我们希望选择

1. 开放哪些设施，以及
2. 使用哪些 (开放) 设施来供应 $$m$$ 为了以最低成本满足某些固定需求

我们引入以下符号:

* 让 $$f_{i}$$ 表示开办设施 $i$ 的 (固定) 成本
* 让 $$c_{ij}$$ 表示把产品从设施 $i$ 送到客户 $j$ 的成本
* 让 $$d_{j}$$ 表示客户的需求
* 让 $$u_{i}$$ 表示设施 $$i$$ 的容量, 对于无限容易设施选址问题, $$u_{i} = \infty$$

**无限容量设施选址问题 (Uncapacitated Facility Location Problem)** 对应的数学规划模型由以下公式给出:

$$
\begin{aligned}
\min \quad & \sum_{i=1}^{n} \sum_{j=1}^{m} c_{ij} d_j y_{ij} + \sum_{i=1}^{n} f_i x_i \\
\text{s.t.} \quad & \sum_{i=1}^{n} y_{ij} = 1 \quad \forall j = 1, \dots, m \\
& \sum_{j=1}^{m} y_{ij} \leq M x_i \quad \forall i = 1, \dots, n \\
& y_{ij} \in \{0, 1\} \quad \forall i = 1, \dots, n \text{ and } j = 1, \dots, m \\
& x_i \in \{0, 1\} \quad \forall i = 1, \dots, n
\end{aligned}
$$

**有限容量设施选址问题 (Capacitated Facility Location Problem)** 有限容量设施选址问题即在无限容量设施选址问题上多考虑了容量问题. 现在每个设施提供的服务是有限的, 其对应的数学规划模型由以下公式给出:

* 现在, $$y_{ij}$$ 代表的是客户 $$m$$ 有多少比例的需求是被设施 $$n$$ 满足的

$$
\begin{aligned}
\min \quad & \sum_{i=1}^{n} \sum_{j=1}^{m} c_{ij} d_j y_{ij} + \sum_{i=1}^{n} f_i x_i \\
\text{s.t.} \quad & \sum_{i=1}^{n} y_{ij} = 1 \quad \forall j = 1, \dots, m \\
& \sum_{j=1}^{m} d_j y_{ij} \leq u_i x_i \quad \forall i = 1, \dots, n \\
& y_{ij} \geq 0 \quad \forall i = 1, \dots, n \text{ and } j = 1, \dots, m \\
& x_i \in \{ 0, 1 \} \quad \forall i = 1, \dots, n
\end{aligned}
$$
