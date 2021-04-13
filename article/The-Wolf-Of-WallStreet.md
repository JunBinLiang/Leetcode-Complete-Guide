



## 前言

今天我们来聊聊**华尔街之狼**(The Wolf of Wall Street)系列，也称 **股票系列**， 在 Leetcode 上有 7 题之多。我们今天将学习其中的几道再次探究动态规划的魅力，希望能帮助大家对 DP 有更深入的理解。
<br/>
**:bulb:注** ： 题目不会按照Leetcode 上的顺序来讲，小编自行对题目的难度和相似度进行了分类，希望能更好的帮助读者去了解

### 我们将学习 : 

 1. [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock)
 2. [  Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii)
 3. [  Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee)
 4. [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii)
 5. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown)

下面直接进入今天的主题，**华尔街之狼**系列。<br/><br/>



## 1. Best Time to Buy and Sell Stock

#### 题意：

>给你一组数组 ``prices=[7,1,5,3,6,4]``。prices[i] 代表在第i天股票的价格。你可以进行买与卖的操作，但你得先买了才能卖。（话说，没有股票在手你怎么卖呢）你最多进行一次买与卖的操作，问你能够赚到的最大收益是多少？<br/><br/>
>从本题的数据例子来看，我们如果在 i=1 天买和在 i=4天卖，我们能够赚到 p[4]-p[1]=5 的收益。这是我们能够做到的最大收益，其它的操作都不能赚的比5多。




### 问题分析 ： 

 - 首先如果我们在第 i 天进行买的操作，那么卖的操作一定得发生在 **prices[i+1 : n]** 里
 - 我们可以尝试枚举买的操作。以 **prices=[7,1,5,3,6,4]** 作为例子，如果我们在p[0] 买，那么卖一定发生在 p[1:5]。同理，如果我们在p[1]买，卖一定发生在p[2:5]。我们可以把所有的 **（买，卖）**  pair 生成出来然后找到收益性最高的那对即可

### :bulb:题解1 ：暴力
```
    public int maxProfit(int[] prices) {
        int maxProfit = 0; //我们可以进行0操作，所以初始是0 而不是 INT_MIN
        for(int i = 0; i < prices.length; i++){
            //在 i 天 进行购买
            for(int j = i + 1; j < prices.length; j++){
                //在 j 天进行出售
                int profit = prices[j] - prices[i];
                maxProfit = Math.max(maxProfit, profit);
            }
        }
        return maxProfit;
    }
```
#### 代码总结：

 - 我们枚举所有的 **(i,j)** ，**i** 代表买，**j** 代表卖，并且**i<j**，但以上暴力代码的时间复杂度是 **O(n^2)**，我们可不可以做的更好呢？

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N^2) 
  - 空间复杂度：O(1)
<br/><br/>

### :bulb:题解2：

 - 如果我们在第 i 天进行买的操作，那么卖的操作一定还是得发生在 **prices[i+1 : n]** 这个定理是不变的
 - 换句话说，对于每个买的操作，**prices[i]**，我们只需要找到 **prices[i+1 : n]** 里最大的数即可
 - 我们可以用一个dp array 去记录，**dp[i]** 表示 **max(prices[i : n])** 
 - 如果我们在 i 这天进行买的操作，他最大的收益就是 **dp[i+1] - prices[i]** (这里我们要注意outbound)
```
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        int n = prices.length;
        int dp[] = new int[n]; 


        dp[n - 1] = prices[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            dp[i] = Math.max(prices[i], dp[i + 1]);
        }
        

        for (int i = 0; i < n - 1; i++) {
            maxProfit = Math.max(maxProfit, dp[i + 1] - prices[i]);
        }

        return maxProfit;
    }
```

#### 代码总结：

 - 如果我们在 i 进行买的操作，卖就发生在  **[i+1 : n]** 里。比起去生成所有的pair，我们只需要知道**prices[i+1 : n]** 的最大值即可，我们可以用一个 **dp** array 去把我们所需要的提前给计算好。**dp[i]** 代表 **prices[i : n]** 的最大值

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N) 
  - 空间复杂度：O(N)
<br/><br/>

### :bulb:题解3：

 - 我们其实可以把空间压缩到 **O(1)**，只要把顺序改改就行
 - 如果我们在 i 天卖，同理，买得发生在 **[0 : i-1]**, 我们这次只需要找  **prices[0 : i-1]** 里最小的即可，我们可以一边走loop一边记录

```
	public int maxProfit(int[] prices) {
        int minSell = prices[0];
        int maxProfit = 0;

        for (int i = 1; i < prices.length; i++) {
            maxProfit = Math.max(maxProfit, prices[i] - minSell);
            minSell = Math.min(minSell, prices[i]);
        }
        return maxProfit;
    }
   ```
#### 代码总结：

 - 同样的思路，我们把顺序调换一下就可以做的更好。我们设计算法的时候应该从多个方向和维度进行思考

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N) 
  - 空间复杂度：O(1)
<br/><br/>
