# 注意事项

由于不同问题对应的参考文献不同, 其对应的数学规划表达式也可能不同 (虽然它们是等价的).

所以在查看具体内容之前请先明确该板块所使用的表达式 (在每个板块最前面和每段代码最前面都有标注)

### 以下是常见的转换方式:

<mark style="color:orange;">**目标函数**</mark> $$min$$ <mark style="color:orange;">**和**</mark> $$max$$ <mark style="color:orange;">**的转化:**</mark> $$min\ c^{T}x \iff max\ - c^{T}x$$



<mark style="color:orange;">**约束条件大于等于和小于等于的转化:**</mark> $$Ax \leq b \iff -Ax \geq -b$$



<mark style="color:orange;">**约束条件等式和不等式的转化:**</mark> $$Ax + y = b,\ y \in \mathbb{R_{+}}^{m \times 1} \iff Ax \leq b$$

* 从右到左的转化思想是: 给每一个约束添加一个辅助变量使得不等式变成等式

