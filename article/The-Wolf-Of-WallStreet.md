






## 前言

今天我们来聊聊**华尔街之狼**(The Wolf of Wall Street)系列，也称 **股票系列**， 在 Leetcode 上有 6 题之多。我们今天将学习其中的几道再次探究动态规划的魅力，希望能帮助大家对 DP 有更深入的理解。
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

 - 首先如果我们在第 **i** 天进行买的操作，那么卖的操作一定得发生在 **prices[i+1 : n]** 里<br/><br/>
 - 我们可以尝试枚举买的操作。以 **prices=[7,1,5,3,6,4]** 作为例子，如果我们在**prices[0]** 买，那么卖一定发生在 **prices[1 : 5]**。同理，如果我们在**prices[1]**买，卖一定发生在**prices[2 : 5]**。我们可以把所有的 **（买，卖）**  pair 生成出来然后找到收益性最高的那对即可

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

 - 我们枚举所有的 **(i,j)** pair，**i** 代表买，**j** 代表卖，并且**i<j**，但以上暴力代码的时间复杂度是 **O(n^2)**，我们可不可以做的更好呢？

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N^2) 
  - 空间复杂度：O(1)
<br/><br/>

### :bulb:题解2：

 - 如果我们在第 i 天进行买的操作，那么卖的操作一定还是得发生在 **prices[i+1 : n]** 这个定理是不变的<br/><br/>
 - 换句话说，对于每个买的操作，**prices[i]**，我们只需要找到 **prices[i+1 : n]** 里最大的数即可<br/><br/>
 - 我们可以用一个dp array 去记录，**dp[i]** 表示 **max(prices[i : n])** <br/><br/>
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

 - 如果我们在 i 进行买的操作，卖就发生在  **prices[i+1 : n]** 里。比起去生成所有的pair，我们只需要知道**prices[i+1 : n]** 的最大值即可，我们可以用一个 **dp** array 去把我们所需要的提前给计算好。**dp[i]** 代表 **prices[i : n]** 的最大值

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N) 
  - 空间复杂度：O(N)
<br/><br/>

### :bulb:题解3：

 - 我们其实可以把空间压缩到 **O(1)**，比起使用一个dp array 去记录，我们可以直接一边走一边记录

```
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int maxSell = prices[n - 1];
        int maxProfit = 0;

        for (int i = n - 2; i >= 0; i--) {
            maxProfit = Math.max(maxProfit, maxSell - prices[i]);
            maxSell = Math.max(maxSell, prices[i]);
        }
        return maxProfit;
    }
```
#### 代码总结：

 - 同理，枚举买，如果在i进行买，卖就发生在**prices[i+1 : n]**，找到**prices[i+1 : n]** 最大的即可，我们可以一边走一边记录最大的从而省略掉array

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N) 
  - 空间复杂度：O(1)
<br/><br/>


## 2. Best Time to Buy and Sell Stock III

>与第一题类似，给你一组数组 ``prices=[3,3,5,0,0,3,1,4]``。prices[i] 代表在第i天股票的价格。你可以进行买与卖的操作，但你得先买了才能卖。（话说，没有股票在手你怎么卖呢）这一次，你最多进行2次买与卖的操作，问你能够赚到的最大收益是多少？<br/><br/>
>如果我们在第四天买第六天卖，和在第七天买第八天卖，我们可以得到 (3-0)+(4-1)=6，这是我们能得到的最大利益


### 问题分析 ：

 - 这次这题升级了一点难度，可以进行两次的交易，但是只要打好了第一题的基础，这题也并不会太难的，我们已经通过了第一题学会了如何计算最多进行一次买卖操作的最大利润，我们将通过已经计算好的一次交易最大利润去计算两次的是多少<br/><br/>
 - 假设我们第一次 **卖** 发生在 **i**，那么我们第一次交易得发生在 **prices[0 : i]**， 而我们第二次交易得发生在 **prices[i+1 : n]**<br/><br/>
 - 我们可以像第一题的 **题解3** 一样，对于第一次交易，我们枚举每一次**卖**，我们只需要再找到 **prices[i+1 : n]** 的一次最大利润操作即可<br/><br/>
 - 对于 **prices[i+1 : n]** 这一段，我们可以枚举**买**，如果买发生在 **prices[i]**,那么卖得发生在 **prices[i+1 : n]**

### :bulb:题解1：
```
    public int maxProfit(int[] A) {
        int n = A.length;
        int maxProfit = 0;

	//dp[i] 代表 prices[i:n] 能得到的最大一次交易利润，也就是我们的第二次操作
		
        int dp[] = new int[n];
        int maxSell = A[n - 1];

        for (int i = n - 2; i >= 0; i--) {
	    //maxSell-A[i] 代表如果我们在i这天进行购买的话的最大利润
            dp[i] = Math.max(dp[i + 1], maxSell - A[i]);
            maxSell = Math.max(maxSell, A[i]);
            maxProfit = Math.max(maxProfit, dp[i]);
        }

        int minBuy = A[0];
        for (int i = 1; i < A.length - 1; i++) {
	    //假设我们第一次卖发生在i,买得发生在prices[0:i-1]
	    //第二次操作发生在prices[i+1 : n]，dp[i+1]表示prices[i+1 : n]这段区间进行一次操作的最大值
            maxProfit = Math.max(maxProfit, dp[i + 1] + (A[i] - minBuy));
            minBuy = Math.min(minBuy, A[i]);
        }

        return maxProfit;
    }
```

#### 代码总结：

 - 我们枚举第一次的卖发生在**i**，那么第一次操作就肯定发生在**prices[0 : i]** 而第二次操作肯定发生在**prices[i+1 : n]**<br/><br/>
 - 我们可以提前对**prices[i+1 : n]** 进行提前处理。**dp[i]** 代表 **prices[i : n]** 最大利润的一次交易，我们可以像第一题的**题解3**一样去枚举**买**<br/><br/>
 - 再枚举第一次交易的卖，如果它发生在**i**，我们能得到的最大利润就是**prices[i] - min(prices[0 : i-1]) + dp[i+1]**

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N) 
  - 空间复杂度：O(N)
<br/><br/>


## 3. Best Time to Buy and Sell Stock with Transaction Fee && Best Time to Buy and Sell Stock II 


#### 题意：
>同样还是给你一组数组代表每一天的股价，这次我们可以进行多次买卖，但是每一次买卖你需要多付一个fee。例如，prices = [1,3,2,8,4,9], fee = 2 <br/><br/>
>我们可以在第1天买第4天卖和第5天买第6天卖，总收益是(8-1-2) + (9-4-2) = 8，这是我们能得到的最大收益。
>这里我们会两题一起讲，因为他们是一样的，Best Time to Buy and Sell Stock II 其实就是fee=0 的情况，如果我们能做出Best Time to Buy and Sell Stock with Transaction Fee，Best Time to Buy and Sell Stock II 就迎刃而解了

### 问题分析 ：

 - 与之前的问题一样，我们可以试着枚举买或者卖, 但是因为这次不只是只有一次操作这么简单，如果我们在 **j** 天进行购买和**i** 天进行卖 (j<i), **prices[0 : j-1]** 可能还存在着其它的交易<br/><br/>
 - 我们试着像第一题的题解1一样先从暴力入手，去试着枚举 **（买，卖）** pair。如果我们在**i**天进行卖和**j**天进行买，他的单次交易 **(singleTransaction)** 能得到的利益是 **prices[i]-prices[j]-fee**。但我们别忘了，我们**prices[0 : j-1]** 还可以进行其它的交易，所以我们可以用一个 **dp** 去记录，**dp[i]** 表示 **prices[0:i]** 能得到的最大收益<br/><br/>
 - 所以如果我们在 **i**天卖和在**j** 天买，那么我们能得到的最大收益就是 **prices[i]-prices[j]-fee+dp[j-1]**<br/><br/>
 - 现在我们剩下的问题就是如何去计算**dp[i]**，如果我们在**i** 天进行卖，**j** 进行买，那么如果我们枚举所有 **j** 的 可能性的话，**dp[i]=max(prices[i] - prices[j]-fee + dp[j-1])**。但是我们别忘了一件事，我们在**i** 这天也可以不进行任何的操作，所以还要跟 **dp[i-1]** 进行比较。综上，**dp[i]=max(dp[i-1], max(prices[i] - prices[j]-fee + dp[j-1]))** <br/>

 

### :bulb:题解1 ：暴力DP
```
 public int maxProfit(int[] prices, int fee) {
   int n = prices.length;
   int dp[] = new int[n];//dp[i]表示 [0:i]的最大收益

   for (int i = 1; i < n; i++) {//枚举i,i是卖的天数
     dp[i] = dp[i - 1];
     for (int j = i - 1; j >= 0; j--) {//j 是买的天数，j<i
       int singleTransaction = Math.max(0, prices[i] - prices[j] - fee);
       //注意outbound
       if (j - 1 >= 0) {
         dp[i] = Math.max(dp[i], singleTransaction + dp[j - 1]);
       } else {
         dp[i] = Math.max(dp[i], singleTransaction);
       }
     }
   }
   return dp[n - 1];
 }
```

#### 代码总结：

 - 暴力的dp，我们还会通过仔细观察 dp 的关系转移去进行深一步的优化

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N^2) 
  - 空间复杂度：O(N)
<br/><br/>

### :bulb:题解2 ：优化DP

 - 我们可以从**dp**的关系转移中进行优化<br/><br/>
 - 从**题解1**我们可以看出 **dp[i]=max(dp[i-1], max(prices[i] - prices[j]-fee + dp[j-1]))**，从这转移式中我们可以发现 **i** 是一个不变量，而 **j** 是变量<br/><br/>
 - 首先，我们设**dp[i]=dp[i-1]**。 我们再仔细的观察一下这个式子 **prices[i] - prices[j]-fee + dp[j-1]**，当我们枚举 **i** 的时候，我们会发现**prices[i] - fee** 是个常数！我们如果把式子重新整理一下，那它就是 **(prices[i] - fee) - (prices[j] - dp[j-1])**。我们要是想整个式子的值越大，变量部分**prices[j] - dp[j-1]** 就得越小 <br/><br/>
 - 如果我们在 **i** 进行**卖**，**dp[i] = (prices[i] - fee) - min(prices[j] - dp[j-1])**，我们可以用一个**min** 去记录 **prices[j] - dp[j-1]**，一边遍历一边update。没错，跟第一题的操作是完全一样的
 

```
public int maxProfit(int[] A, int fee) {
  int n = A.length;
  int dp[] = new int[n];
  int min = A[0];

  for (int i = 1; i < A.length; i++) {
    int cur = A[i] - fee;
    dp[i] = dp[i - 1];
    dp[i] = Math.max(dp[i], cur - min);
    min = Math.min(min, A[i] - dp[i - 1]); 
  }
  return dp[n - 1];
}
```

#### 代码总结：

 - **DP** 其实就是一种关系(式子)的转化，当我们求出他的基本关系的时候，我们可以看看能不能通过它的关系进行优化

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N) 
  - 空间复杂度：O(N)
<br/><br/>


## 4. Best Time to Buy and Sell Stock with Cooldown
#### 题意：
>给你一个数组prices = [1,2,3,0,2]，你可以进行多次交易，但每完成一次交易得有一个cooldown，不能连续做交易 <br/><br/>
>按照以上的数据，如果我们按这样的操作[buy, sell, cooldown, buy, sell]
>我们能够得到利益 (2 - 1) + (2 - 0) =3，这是我们能够得到的最大利益

### 问题分析 ：

 - 如果你会了第三题的解法，你会发现这题与上一题其实是异曲同工<br/><br/>
 - 因为有多次交易的关系，我们可以像上一题那样，使**dp[i]** 表示 **prices[0 : i]** 的最大收益。如果我们在 **i** 这天进行**卖** 和 **j** 这天进行**买**，我们能得到的收益就是 **prices[i]-prices[j] + dp[j-2]**<br/><br/>
 - 剩下的问题就是define **dp[i]**。 我们首先**dp[i]=dp[i-1]**，因为在**i**这天我们可以不进行任何操作。然后我们要找的就是 **max(prices[i] - prices[j] +dp[j-2])**<br/><br/>
 - 和上一题一样，当我们在**i** 这天时，**prices[i]** 是个常数。 我们只需要找到最大的 **(-prices[j]+dp[j-2])** 即可，我们可以像上题一样一边计算一边记录


### :bulb:题解1：
```
public int maxProfit(int[] A) {
  if (A.length == 0) return 0;
  int dp[] = new int[A.length];

  //A[i]-A[j]+dp[j-2]
  
  int max = -A[0];
  for (int i = 1; i < A.length; i++) {
    dp[i] = Math.max(dp[i - 1], A[i] + max);
    if (i - 2 >= 0) {
      max = Math.max(max, dp[i - 2] - A[i]);
    } else {
      max = Math.max(max, -A[i]);
    }
  }
  return dp[A.length - 1];
}
```

#### 代码总结：

 - 与上一题是异曲同工。我们首先把**dp** 的关系式找出来，然后根据这关系式再进行进一步的优化

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(N) 
  - 空间复杂度：O(N)
<br/><br/>

## 总结

今天给大家总结了5题的 **股票系列** 题目。大家可以从第一题看出我们是如何一步一步分析问题然后将问题给简单化的。

我们可以先用枚举的方式把问题的大概给解出来，然后通过观察看看哪一些地方是可以优化的！

三四题我们还学习了如何对**DP**进行优化。**DP** 就是一种关系的转换，在这转换过程中有时会很复杂，但有时又会有规律。如果我们在这过程中发现可优化的地方，我们可以对他进行再优化！
