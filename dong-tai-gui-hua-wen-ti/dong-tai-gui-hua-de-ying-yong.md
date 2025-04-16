# 动态规划的应用

### **背包问题 (Knapsack Problem)**

给定一组物品, 每个物品都有一个重量和一个价值. 决定将哪些物品放在背包中, 使得总重量小于或等于给定的重量限制, 并且使总价值最大的问题被称作背包问题

**0-1背包问题:** 每件物品有且只有一件

1. 定义 $$DP[i, w]$$ 为: 前 $$i$$ 件物品放入一个最多能装重量为 $$w$$ 的背包中, 可以获得的最大价值
2. 对于 $$DP[i, w]$$ 这个子问题, 我们考虑最后一个物品 $$i$$ 的放入策略, 容易观察到
   1. 如果 $$w < weight(i)$$, 那么我们无法放入物品 $$i$$, 则该子问题等同于把前 $$i-1$$ 个物品装入重量为 $$w$$ 的背包中可以获得的最大价值
   2. 如果 $$w \geq weight(i)$$, 那么我们可以考虑放入物品 $$i$$
      1. 如果不选择放入物品 $$i$$, 则该子问题一样等同于把前 $$i-1$$ 个物品装入重量为 $$w$$ 的背包中可以获得的最大价值
      2. 如果选择放入物品 $$i$$, 那么前 $$i-1$$ 个物品的重量必须小于 $$w - weight(i)$$, 则该子问题等同于把前 $$i-1$$ 个物品装入重量为 $$w - weight(i)$$ 的背包中可以获得的最大价值加上物品 $$i$$ 的价值
3. (初始条件)
   1. 如果背包载重上限为 $$0$$, 则无论选取什么物品, 可以获得的最大价值一定是 $$0$$, 即 $$DP[i, 0] = 0$$
   2. 无论背包载重上限是多少, 前 $$0$$ 件物品所能获得的最大价值一定为 $$0$$, 即 $$DP[0, w] = 0$$

```python
def zeroOnePackMethod1(weight, value, W):
    size = len(weight)
    dp = [[0 for _ in range(W + 1)] for _ in range(size + 1)]
    
    for i in range(1, size + 1):
        for w in range(W + 1):
            if w < weight[i - 1]:
                dp[i][w] = dp[i - 1][w]
            else:
                dp[i][w] = max(dp[i - 1][w], dp[i - 1][w - weight[i - 1]]
                           + value[i - 1])
                
    return dp[size][W]

items_weight = [2, 1, 3, 2]
items_values = [3, 2, 4, 2]
max_weight = 5

print("背包可容纳物品的最大价值: "
      + str(zeroOnePackMethod1(items_weight, items_values, max_weight)))
```

```
背包可容纳物品的最大价值: 7
```
