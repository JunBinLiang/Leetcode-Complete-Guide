# <center>石子游戏系列</center>

今天我们将学习Leetcode里的 **Stone Game** 系列。 我们的目的是将通过学习这一系列的问题使我们对 **Dynamic Programming** , 也就是我们熟悉的**DP (动态规划)** 有一个更好的了解！<br/><br/>我们使用**Top Down** 和 **Bottom Up** 两种方式去理解 **DP** 
<br/>

动态规划是什么 (DP)？
 - 它是一种算法上的优化
 - 它是一种状态的压缩
 - 它是cache, 用来避免重复的计算



#  博弈入门

在我们学习**石子游戏**系列之前，我们需要对**博弈**有简单的了解 <br/>(这文章的目的并不是为了学习博弈论，而是通过简单的游戏系列来学习**DP**，但是我们需要一些基础来开始)。<br/>

<br/>
我们可以用我们熟悉的猜拳来开始:<br/>

 - 假设小六和小丁在玩猜拳，如果小六已经猜到了小丁会出布，那么小六肯定会出剪刀，因为剪刀能使小六获胜。但是，如果小丁已经猜到小六会猜到自己出布，那么小丁会不会改变策略呢 ？如果小六又猜到了小丁已经猜到了自己猜到了小丁的想法 (到这里已经开始有点乱了)，他又会怎么做呢？
 - 从以上我们可以看出，两个人的**猜**在不停的进行博弈，在这过程中都希望选出一个对自己最有利 (**optimal**) 的方案

<br/>
有了简单的了解之后让我们来看看我们的石子游戏吧！



## Stone Game I


题意：给你一组数组 **A=[5,3,4,5]** 小六(先手)和小丁轮流从里面取数字，**但**，两人只能拿第一个或者最后一个数字，取完之后这数字会从**A**中移除，问谁是最后的赢家如果两个人每次都采取对自己最有利的方案。分数最多者为胜利者。<br/>

分析：

 - 对于每个player来说，他都有两种选择，以**第一轮**为例子，如果小六选了A[0], 那么他会留下 **[3,4,5]**  给小丁做选择，如果他选的是A[3], 他会留下 **[5,3,4]** 给小丁做选择。
 - 如果我们选择**暴力**的话，我们可以把所有的可能性都列举出来。当某一个player选掉一个数字后，他可以把剩下的数字递归给下一位player，当所有数字取完时，游戏结束
 - 这里我们可以得到一个观察，因为数字只能取**第一个**或者**最后一个**，我们可以用一个区间 **[l,r]** 去表达剩下的数字。如果我们取第一个，新的A会变成[l+1,r]。如果我们取最后一个，新的A是[l,r-1]  (这就是我们所谓的**状态压缩**，用两个数字来表达数组的状态，这里的状态是指它还剩下那些数字)
 - 除此之外，我们还需要一个额外的 **player** variable 去记录当前是谁的回合 (不同的player会用不同的决策)
<br/>
 下面并不是完结的代码，我们接下来还要对每个人采用的**决策**(博弈)进行分析
 ```
//我们可以用以下来表达简单的递归模式
public int play(int A[],int l,int r,int player){ 
	if (l > r) {//如果都取完了的情况
		 return 0；
	}
	int nextPlayer = (player + 1) % 2;
	int score = 0;

	score = Math.max(score, A[l] + play(A,l+1,r,nextPlayer ));//如果取第一个数字
	score = Math.max(score, A[r] + play(A,l,r-1,nextPlayer ));//如果取最后一个数字

    return score;
} 
```




- 我们首先会重新定义**play**函数，**play** 函数的返回值是**小六能拿的分数**。如果最终这个分数在总分数一半以上的话，小六能获胜，反之，小丁获胜或平手。
- 如果我是**小六**，对于返回来的分数，有两个选择。身为小六，如果他想要拿到最多的分，那他是不是得选择做大的那一个？
- 如果我是**小丁**，同样有两个选择。但跟小六不一样的是，小丁会选择返回一个最小的数给小六因为她不想让小六获胜 (**提醒**：play函数返回的是小六的分数)
 
<br/>

最终我们的代码将能用**dp[l][r][player]** 去表达我们的压缩状态，**[l,r]** 就如之前所说，表示当前剩下的数字的范围，**player**代表当前是谁的回合，我们用0代表小六，1代表小丁。

**完整代码 1**<br/>
**时间**: O(n^2)<br/>
**空间**: O(n^2)<br/>

 ```
class Solution {  
	 int dp[][][];  
	 public boolean stoneGame(int[] A) {  
	     dp=new int[A.length][A.length][2];  
		    int sum = 0;  
	     for(int i : A){  
	         sum += i;  
	     }    
	  
	     int maxScore = play(A,0,A.length-1,0);  
	     return maxScore * 2 > sum;  
	  }  
  
      public int play(int A[],int l,int r,int player){  
        if(l > r){  //数组为空
            return 0;  
        }  
        
        if(dp[l][r][player]!=0){//如果是已经访问过的状态，我们不需要再重新计算  
            return dp[l][r][player];  
        }  
  
        int nextPlayer = (player + 1) % 2; //下一个player是谁？我们用0代表小六，1代表小丁 
        int score = 0;  
  
        if(player == 0){ 
	        //小六：对于返回的分数，我要取最大的那一个 
            score=Math.max(score,A[l]+play(A,l+1,r,nextPlayer));  
            score=Math.max(score,A[r]+play(A,l,r-1,nextPlayer));  
        }  
        else{  
	        //小丁：hmm，为了不让小六赢，我得返回一个最小的数字给小六
            score=Math.min(play(A,l+1,r,nextPlayer),play(A,l,r-1,nextPlayer));  
        }  
  
        dp[l][r][player] = score;//对状态的值的储存
        return score;  
      }  
}
```

以上是**TopDown** 的DP 方式，非常的好实现，并且从时间空间复杂度来说与**BottomUp**并没有什么区别。但是它的问题是因为用的是递归，所以会对**runtime stack** 有很大的要求。我们再来看看**BottomUp**是怎么写的，我们可以透过**TopDown**已经写好了的关系来进行一点简单的改变就行了。
<br/>
我们已知
 - **dp[l][r][0]=Math.max(A[l]+dp[l+1][r][1],A[r]+dp[l][r-1][1])**
 - **dp[l][r][1]=Math.min(dp[l+1][r][0],dp[l][r-1][0])**
 - 我们需要注意的只剩下dp 填表的顺序。我们需要保证求当前的状态时，之前的状态已经是记录好了的
 - 我们会按 (0,0),(1,1),(2,2)  ... (n,n), (0,1),(1,2),(2,3) ...  这样的顺序。


**完整代码 2**<br/>
```
class Solution {  
    public boolean stoneGame(int[] A) {  
        int dp[][][] = new int[A.length][A.length][2];  
        int sum = 0;  
		for (int i = 0; i < A.length; i++) {  
		     sum += A[i];  
		}  
  
        for (int i = 0; i < dp.length; i++) {  
            dp[i][i][0] = A[i];  
            dp[i][i][0] = 0;  //对base的初始化
        }  
  
        for (int i = 1; i < dp.length; i++) {  
            for (int l = 0; l + i < dp.length; l++) {  
                dp[l][l + i][0] = Math.max(A[l] + dp[l + 1][l + i][1], A[l + i] + dp[l][l + i - 1][1]);  
                dp[l][l + i][1] = Math.min(dp[l + 1][l + i][0], dp[l][l + i - 1][0]);  
             }  
        }  
  
        return dp[0][A.length - 1][0] * 2 > sum;  
     }  
}
```

<br/><br/>

