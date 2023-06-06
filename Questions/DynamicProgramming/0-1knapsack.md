---
title: 0-1knapsack
date: 2023/06/05
author: Runming
type: 题解
---
[代码随想录](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html#%E4%BA%8C%E7%BB%B4dp%E6%95%B0%E7%BB%8401%E8%83%8C%E5%8C%85)

## Tag
#0-1knapsack, #dp

## 审题（关键词） 
有n件物品和一个最多能背重量为w 的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。每件物品只能用一次，求解将哪些物品装入背包里物品价值总和最大。

## 初始思路  
- 确定dp数组以及下标的含义
  - 对于背包问题，有一种写法， 是使用二维数组，即dp[i][j] 表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少。
- 确定递推公式
  - 再回顾一下dp[i][j]的含义：从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少。

- 那么可以有两个方向推出来dp[i][j]，
  1. 放物品
  2. 不放物品
  3. dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

- dp数组如何初始化
  - 关于初始化，一定要和dp数组的定义吻合，否则到递推公式的时候就会越来越乱。
  - 根据公式可知，dp[i] 由dp[i-1]而来，所以0一定要初始化
    - dp[0][j]，即
      - j < weight[0]的时候，dp[0][j] 应该是 0，因为背包容量比编号0的物品重量还小。
      - j >= weight[0]时，dp[0][j] 应该是value[0]，因为背包容量放足够放编号0物品。
      - 其他下标初始为什么数值都可以，因为都会被覆盖。
## 考点  
01 背包
## 解法1
```java
public class BagProblem {
    public static void main(String[] args) {
        int[] weight = {1,3,4};
        int[] value = {15,20,30};
        int bagSize = 4;
        testWeightBagProblem(weight,value,bagSize);
    }

    /**
     * 动态规划获得结果
     * @param weight  物品的重量
     * @param value   物品的价值
     * @param bagSize 背包的容量
     */
    public static void testWeightBagProblem(int[] weight, int[] value, int bagSize){

        // 创建dp数组
        int goods = weight.length;  // 获取物品的数量
        int[][] dp = new int[goods][bagSize + 1];

        // 初始化dp数组
        // 创建数组后，其中默认的值就是0
        for (int j = weight[0]; j <= bagSize; j++) {
            dp[0][j] = value[0];
        }

        // 填充dp数组
        for (int i = 1; i < weight.length; i++) {
            for (int j = 1; j <= bagSize; j++) {
                if (j < weight[i]) {
                    /**
                     * 当前背包的容量都没有当前物品i大的时候，是不放物品i的
                     * 那么前i-1个物品能放下的最大价值就是当前情况的最大价值
                     */
                    dp[i][j] = dp[i-1][j];
                } else {
                    /**
                     * 当前背包的容量可以放下物品i
                     * 那么此时分两种情况：
                     *    1、不放物品i
                     *    2、放物品i
                     * 比较这两种情况下，哪种背包中物品的最大价值最大
                     */
                    dp[i][j] = Math.max(dp[i-1][j] , dp[i-1][j-weight[i]] + value[i]);
                }
            }
        }

        // 打印dp数组
        for (int i = 0; i < goods; i++) {
            for (int j = 0; j <= bagSize; j++) {
                System.out.print(dp[i][j] + "\t");
            }
            System.out.println("\n");
        }
    }
}
```

## 使用滚动数组优化
其实可以发现如果把dp[i - 1]那一层拷贝到dp[i]上，表达式完全可以是：  
dp[i][j] = max(dp[i][j], dp[i][j - weight[i]] + value[i]);

**核心理解：当前的值是由之前的一层拷贝而来**

1. 确定dp数组的定义
在一维dp数组中，dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]。
一维dp数组的递推公式

2. dp[j]可以通过dp[j - weight[i]]推导出来，  
   dp[j - weight[i]]表示容量为j - weight[i]的背包所背的最大价值。
    此时dp[j]有两个选择，
    1. 一个是取自己dp[j] 
         - 相当于 二维dp数组中的dp[i-1][j]，即不放物品i，
    2. 一个是取dp[j - weight[i]] + value[i]，
         - 即放物品i，指定是取最大的，毕竟是求最大价值，

3. 一维dp数组如何初始化
背包容量为0所背的物品的最大价值就是0。
其他也可以为0，因为之后会被下一层覆盖

### Questions
1. 为什么要倒序，
   - 抓住核心：
     - 当前的值是由之前的一层拷贝而来，
   - 如果顺序的话，则直接更新了本层，之后由dp[j - weight[i]]获得的值是本层已经被更新过的值，比如：
        ```
        dp[1] = dp[1 - weight[0]] + value[0] = 15

        dp[2] = dp[2 - weight[0]] + value[0] = 30
        ```
   - 所以需要倒序遍历
        ```
        dp[2] = dp[2 - weight[0]] + value[0] = 15
        dp[2 - weight[0]]就会是上一层没被更新的0.
        ```
2. 可否物品for循环和背包for循环调换位置：
   - 不可以更换，
     - 如果更换之后，每个背包重量只能配一个物品
## solution2
```java
    public static void main(String[] args) {
        int[] weight = {1, 3, 4};
        int[] value = {15, 20, 30};
        int bagWight = 4;
        testWeightBagProblem(weight, value, bagWight);
    }

    public static void testWeightBagProblem(int[] weight, int[] value, int bagWeight){
        int wLen = weight.length;
        //定义dp数组：dp[j]表示背包容量为j时，能获得的最大价值
        int[] dp = new int[bagWeight + 1];
        //遍历顺序：先遍历物品，再遍历背包容量
        for (int i = 0; i < wLen; i++){
            for (int j = bagWeight; j >= weight[i]; j--){
                dp[j] = Math.max(dp[j], dp[j - weight[i]] + value[i]);
            }
        }
        //打印dp数组
        for (int j = 0; j <= bagWeight; j++){
            System.out.print(dp[j] + " ");
        }
    }
``` 
