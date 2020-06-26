---
layout: post
title:  "LeetCode学习之动态规划"
date:   2018-06-14
categories: 算法学习  
tags: 算法学习

---

* content
{:toc}


本文为LeetCode中关于动态规划的题目学习

#  动态规划详解

动态规划（英语：Dynamic programming，简称 DP）是一种在数学、管理科学、计算机科学、经济学和生物信息学中使用的，通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。

动态规划常常适用于有重叠子问题和最优子结构性质的问题，动态规划方法所耗时间往往远少于朴素解法。

动态规划背后的基本思想非常简单。大致上，若要解一个给定问题，我们需要解其不同部分（即子问题），再根据子问题的解以得出原问题的解。动态规划往往用于优化递归问题，例如斐波那契数列，如果运用递归的方式来求解会重复计算很多相同的子问题，利用动态规划的思想可以减少计算量。

通常许多子问题非常相似，为此动态规划法试图仅仅解决每个子问题一次，具有天然剪枝的功能，从而减少计算量：一旦某个给定子问题的解已经算出，则将其记忆化存储，以便下次需要同一个子问题解之时直接查表。这种做法在重复子问题的数目关于输入的规模呈指数增长时特别有用。

动态规划问题的⼀般形式就是求最值。动态规划其实是运筹学的⼀种最优化⽅法，只不过在计算机问题上应⽤⽐较多，⽐如说让你求最⻓递增⼦序列呀，最⼩编辑距离呀等等。既然是要求最值，核⼼问题是什么呢？求解动态规划的核⼼问题是穷举。因为要求最值，肯定要把所有可⾏的答案穷举出来，然后在其中找最值呗。动态规划就这么简单，就是穷举就完事了？我看到的动态规划问题都很难
啊！

⾸先，动态规划的穷举有点特别，因为这类问题存在「重叠⼦问题」，如果暴⼒穷举的话效率会极其低下，所以需要「备忘录」或「DP	table」来优化穷举过程，避免不必要的计算。

⽽且，动态规划问题⼀定会具备「最优⼦结构」，才能通过⼦问题的最值得到原问题的最值。

另外，虽然动态规划的核⼼思想就是穷举求最值，但是问题可以千变万化，穷举所有可⾏解其实并不是⼀件容易的事，只有列出正确的「状态转移⽅程」才能正确地穷举。
以上提到的**重叠⼦问题、最优⼦结构、状态转移⽅程**就是动态规划三要素。具体什么意思等会会举例详解，但是在实际的算法问题中，写出状态转移⽅程是最困难的，这也就是为什么很多朋友觉得动态规划问题困难的原因，现在来提供思维框架，辅助我们思考状态转移⽅程：
	**明确「状态」**	->	**定义	dp	数组/函数的含义**	->	**明确「选择」**->	**明确	base case**。

## LeetCode题目集合

#### [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

难度简单63收藏分享切换为英文关注反馈

输入一个整型数组，数组里有正数也有负数。数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

**示例1:**

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

**提示：**

- `1 <= arr.length <= 10^5`
- `-100 <= arr[i] <= 100`

**代码:**

```c++
int maxSubArray(vector<int>& nums) {
    /**
        dp[n] = dp[n-1] + nums[n]   (dp[n-1] + nums[n] >= nums[n])
              = nums[n]             (dp[n-1] + nums[n] < nums[n])
        dp[0] = nums[0]     
    **/
    if(nums.size() == 0) return 0 ;
    int res = nums[0] , dpN = nums[0];
    for(int i = 1 ; i < nums.size() ; i++){
        dpN = dpN + nums[i] >= nums[i] ? dpN + nums[i] : nums[i];
        res = max(dpN , res);
    }
    return res;
}
```

 [303. 区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/)

难度简单163收藏分享切换为英文关注反馈

给定一个整数数组  *nums*，求出数组从索引 *i* 到 *j* (*i* ≤ *j*) 范围内元素的总和，包含 *i, j* 两点。

**示例：**

```
给定 nums = [-2, 0, 3, -5, 2, -1]，求和函数为 sumRange()

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

**说明:**

1. 你可以假设数组不可变。
2. 会多次调用 *sumRange* 方法。

**代码：**

```c++
vector<int> vecSum;
NumArray(vector<int>& nums) {
    vecSum.resize(nums.size() + 1);
    vecSum[0] = 0;
    for(int i = 0 ; i < nums.size() ; i++)
        vecSum[i+1] = vecSum[i] + nums[i];
}

int sumRange(int i, int j) {
    return vecSum[j+1] - vecSum[i]; 
}
```

#### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

难度简单1095收藏分享切换为英文关注反馈

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

**示例 1：**

```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```

**示例 2：**

```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

**代码：**

```c++
int climbStairs(int n) {
	/**
    dp(n) = dp(n-1) + dp(n-2)
    dp(1) = 1   dp(2) = 2
	**/
    if(n<=0) return 0;
    if(n<=2) return n;
    int dpM = 1 , dpN = 2;
    for(int i = 2 ; i < n ; i++){
        int tmp = dpM + dpN;
        dpM = dpN;
        dpN = tmp;
    }
    return dpN;
}
```

### 股票购买问题

针对股票购买问题，将题意进行大概总结。

1. 只能进行一次交易或者交易次数不限
2. 交易有冷冻期或者手续费等额外条件

现在，试图找到对应的**状态**和**选择**：

1. 每天都有三种选择：买入、卖出、无操作。针对不同的题意，不是每天都可以做这三种选择的。

   比如：sell必须在buy之后，buy必须在sell之后。而无操作分为两种：buy之后无操作，sell之后不操作。在有交易次数限制的情况下，buy必须在剩余次数>0的情况。

2. 这个问题的状态有三个：分别是  天数、允许交易的最大次数、当前持有的状态（1表示持有股票  0表示没有持有）

   然后我们可以使用一个三维数组来装下这几种状态的全部组合：

   ~~~~
   dp[i][k][0]
   第i天，不持有股票，允许交易的最大次数为K
   dp[i][k][1]
   第i天，持有股票，允许交易的最大次数为K
   ~~~~

   而我们想求的最终答案是dp[n] [k] [0] ，即最后一天，最多允许K次交易，最多获得多少利润。

那么，可以来看一下状态转移方程：

~~~~
dp[i][k][0] = max(dp[i-1][k][0] , dp[i-1][k][1] + prices[i])
今天没有持有股票的两种可能：
1：昨天没有持有，今天没有任何操作
2：昨天持有股票，今天将股票卖出

dp[i][k][1] = max(dp[i-1][k][1] , dp[i-1][k-1][0] - prices[i])
今天持有股票的两种可能：
1：昨天持有，今天没有任何操作
2：昨天没有持有股票，今天将买入股票
~~~~



#### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。

 

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2:**

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**代码:**

```c++
int maxProfit(vector<int>& prices) {
    /**
    只能买入一次
    dp(n)(0) 第n天剩余买入次数为0
    dp(n)(0) = max(dp(n-1)(0) , dp(n-1) + prices(n-1))
    dp(n)(1) = max(dp(n-1)(1) , -prices(1))
    dp(0)(0) = 0    dp(0)(1) = -prices(0)
    **/

    if(prices.size() <= 1)
        return 0;
    int nTransNum = 1 , nSize = prices.size();
    vector< vector<int> > vecDP(nSize , vector<int>(2));
    vecDP[0][0] = 0; // rset
    vecDP[0][1] = -prices[0]; //  buy
    for(int i = 1 ; i < nSize ; i++){
        vecDP[i][0] = max(vecDP[i-1][0] , vecDP[i-1][1] + prices[i]);
        vecDP[i][1] = max(vecDP[i-1][1] , -prices[i]);
    }
    return vecDP[nSize-1][0];
}
```

#### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

难度简单748收藏分享切换为英文关注反馈

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

**示例 2:**

```
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例 3:**

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**代码:**

```c++
int maxProfit(vector<int>& prices) {
    // dp[i][0] = max(dp[i-1][0] , dp[i-1][1] + prices[i])
    // 第i天没有股票两种情况：第i-1天没有股票 第i天不操作；第i-1天有股票 ， 第i天卖掉
    // dp[i][1] = max(dp[i-1][1] , dp[i-1][0] - prices[1]);
    // 第i天有股票两种情况：第i-1天有股票，第i天不操作；第i-1天没有股票，第i天买入
    // base case 
    int nLen = prices.size() , dpM0 = 0 , dpM1 = 0 , dpN0 =0 , dpN1 = 0 ;
    if(nLen <= 1) return 0;
    dpM0 = 0 , dpM1 = -prices[0];
    for(int i = 1 ; i < nLen ; i++){
        dpN0 = max(dpM0 , dpM1 + prices[i]);
        dpN1 = max(dpM1 , dpM0 - prices[i]);
        dpM0 = dpN0;
        dpM1 = dpN1;
    } 
    return dpN0;
}
```

#### [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

难度中等345收藏分享切换为英文关注反馈

给定一个整数数组，其中第 *i* 个元素代表了第 *i* 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

- 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
- 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

**示例:**

```
输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

**代码:**

```c++
int maxProfit(vector<int>& prices) {
    /*
            DP[i][0] : 第i天不持有股票
            DP[i][0] =max(DP[i-1][1] + prices[i] , DP[i-1][0])
            DP[i][1] : 第i天持有股票
            DP[i][1] = max(DP[i-2][0] - prices[i] , DP[i-1][1])
            base case 
            DP[0][0] = 0    DP[0][1] = -prices[0]
    */
    int nLen = prices.size();
    if(nLen <= 1)
        return 0 ;
    int dpM0 = 0 , dpN0 = 0 , dpN1 = -prices[0];
    for(int i = 1; i < nLen ; i++){
        int tmp0 = dpN0;
        dpN0 = max(dpN1 + prices[i] , dpN0);
        dpN1 = max(dpM0 - prices[i] , dpN1);
        dpM0 = tmp0;
    }
    return dpN0;
}
```

#### [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

难度中等190收藏分享切换为英文关注反馈

给定一个整数数组 `prices`，其中第 `i` 个元素代表了第 `i` 天的股票价格 ；非负整数 `fee` 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

**注意：**这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

**示例 1:**

```
输入: prices = [1, 3, 2, 8, 4, 9], fee = 2
输出: 8
解释: 能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

**注意:**

- `0 < prices.length <= 50000`.
- `0 < prices[i] < 50000`.
- `0 <= fee < 50000`.

**代码:**

```c++
int maxProfit(vector<int>& prices, int fee) {
    // dp[i][0] = max(dp[i-1][0] , dp[i-1][1] + prices[i]-fee)
    // 第i天 没有股票    第i-1天没有股票 第i天不操作     第i-1天有股票 ， 第i天卖掉
    // dp[i][1] = max(dp[i-1][1] , dp[i-1][0] - prices[1]);
    // 第i天有股票     第i-1天有股票，第i天不操作    第i-1天没有股票 第i天买入
    // base case 
    int nLen = prices.size() , dpM0 = 0 , dpM1 = 0 , dpN0 =0 , dpN1 = 0 ;
    if(nLen <= 1) return 0;
    dpM0 = 0 , dpM1 = -prices[0];
    for(int i = 1 ; i < nLen ; i++){
        dpN0 = max(dpM0 , dpM1 + prices[i]-fee);
        dpN1 = max(dpM1 , dpM0 - prices[i]);
        dpM0 = dpN0;
        dpM1 = dpN1;
    } 
    return dpN0;
}
```

#### [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

难度困难235收藏分享切换为英文关注反馈

给定一个数组，它的第 *i* 个元素是一支给定的股票在第 *i* 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 **k** 笔交易。

**注意:** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```
输入: [2,4,1], k = 2
输出: 2
解释: 在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```

**示例 2:**

```
输入: [3,2,6,5,0,3], k = 2
输出: 7
解释: 在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
```

**代码:**

```c++
    int maxProfit_KUnlimited(vector<int>& prices) {
    // dp[i][0] = max(dp[i-1][0] , dp[i-1][1] + prices[i])
    // 第i天 没有股票    第i-1天没有股票 第i天不操作     第i-1天有股票 ， 第i天卖掉
    // dp[i][1] = max(dp[i-1][1] , dp[i-1][0] - prices[1]);
    // 第i天有股票     第i-1天有股票，第i天不操作    第i-1天没有股票 第i天买入
    // base case 
    int nLen = prices.size() , dpM0 = 0 , dpM1 = 0 , dpN0 =0 , dpN1 = 0 ;
    if(nLen <= 1) return 0;
    dpM0 = 0 , dpM1 = -prices[0];
    for(int i = 1 ; i < nLen ; i++){
        dpN0 = max(dpM0 , dpM1 + prices[i]);
        dpN1 = max(dpM1 , dpM0 - prices[i]);
        dpM0 = dpN0;
        dpM1 = dpN1;
    } 
    return dpN0;
}

int maxProfit(int k, vector<int>& prices) {
    /**
        dp[i][k][0] = max( dp[i-1][k][0] , dp[i-1][k][1] + prices[i])
        第i天不持有股票     第i-1天不持有股票且第i天不操作     第i-1天持有股票第i天卖出
        dp[i][k][1] = max( dp[i-1][k][1] , dp[i-1][k-1][0] - prices[i])
        第i天持有股票      第i-1天持有股票且第i天不做操作      第i-1天不持有股票且第i天买入
    **/
    int n = prices.size();
    if(n==0) return 0;
    //注意这里的次序，如果放在新建dp的后面，则会超过内存啊啊啊
    if(k >= n/2){//如果k 大于 n/2 则和k无限制一样
        return maxProfit_KUnlimited(prices);
    }
    int dp[n][k+1][2];
    memset(dp , 0 , sizeof(dp));
    //初始化 base case 
    for(int j=1;j<=k;j++){
        dp[0][j][1]=-prices[0];
    }
    for(int i=1;i<n;i++){
        for(int j=1;j<=k;j++){
            dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1]+prices[i]);
            dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0]-prices[i]);
        }
    }
    return dp[n-1][k][0];
}
```