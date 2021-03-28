# <h1 align="center"><b>石子游戏系列:</b><br><h1>

## 主题：
今天我们将学习 Leetcode 里的 **Stone Game 系列**。 我们的目的是将通过学习这一系列的问题使我们对 **``Dynamic Programming``**, 
也就是我们熟悉的 **DP (动态规划)** 有一个更好的了解！<br/>

**我们将会使用 ``Top Down`` 和 ``Bottom Up`` 两种方式去理解 `DP`**

#### 动态规划是什么 (DP)？
 - 它是一种算法上的优化
 - 它是一种状态的压缩
 - 它是``cache``, 用来避免重复的计算


##  博弈入门
在我们学习**石子游戏**系列之前，我们需要对**博弈**有简单的了解 <br/>
这文章的目的并不是为了学习博弈论，而是通过简单的游戏系列来**学习 DP**，但是我们需要一些基础来开始。<br/>
<br/>

**_我们可以用我们熟悉的猜拳来开始 :_** <br/>

 > 假设小六和小丁在玩猜拳，如果小六已经猜到了小丁会出布，那么小六肯定会出剪刀，因为剪刀能使小六获胜。<br/>
 > 但是，如果小丁已经猜到小六会猜到自己出布，**_那么小丁会不会改变策略呢_** ？<br/>
 > 如果小六又猜到了小丁已经猜到了自己猜到了小丁的想法 (到这里已经开始有点乱了)，**_他又会怎么做呢_** ？。<br/>
 
 **从以上我们可以看出，两个人的``猜``在不停的进行博弈，在这过程中都希望选出一个对自己最有利 (``OPTIMAL``) 的方案**

<br/>

**_有了简单的了解之后让我们来看看我们的石子游戏吧！_**

--- 
---

## <h2 align="center"><b> Stone Game I </b><br><h2>

>**题意**：给你一组数组 **``A=[5,3,4,5]``** 小六 _(先手)_ 和小丁轮流从里面取数字，**``但``**，两人只能拿第一个或者最后一个数字，
>取完之后这数字会从 **``A``** 中移除，问谁是最后的赢家如果两个人每次都采取对自己最有利的方案。**分数最多者为胜利者**。<br/>

#### 分析：
 - 对于每个 _player_ 来说，他都有**两种选择** _(以**第一轮**为例)_:
 	1. 如果小六选了 `A[0]` , 那么他会留下 **``[3,4,5]``**  给小丁做选择
 	2. 如果他选的是 `A[3]` , 他会留下 **``[5,3,4]``** 给小丁做选择。
 - 如果我们选择**暴力**的话，我们可以把所有的可能性都列举出来。当某一个 ``player`` 选掉一个数字后，他可以把剩下的数字递归给下一位 ``player``，当所有数字取完时，游戏结束
 - 这里我们可以得到一个观察，因为数字**只能**取**第一个**或者**最后一个**，我们可以用一个区间 **``[l,r]``** 去表达剩下的数字。如果我们取第一个，新的 **``A``** 会变成 `[l+1,r]` 。
   如果我们取最后一个，新的 **``A``** 是 `[l,r-1]`  (这就是我们所谓的**状态压缩**，用两个数字来表达数组的状态，这里的状态是指它还剩下哪些数字)
 - 除此之外，我们还需要一个额外的 **``player``** _(variable)_ 去记录当前是谁的回合 (不同的 ``player`` 会用不同的决策)
<br/>

**_下面并不是完结的代码。我们还需要对每个人采用的 ``决策 (博弈)`` 进行分析_**
 
 ```java
 
/* 我们可以用以下来表达状态变化的简单的递归模式 */

public int play(int A[],int l,int r,int player){ 
	if (l > r) {                   //如果都取完了的情况 
		return 0；
	}
	int nextPlayer = (player + 1) % 2;
	int score = 0;

	score = Math.max(score, A[l] + play(A,l+1,r,nextPlayer )); //如果取第一个数字
	score = Math.max(score, A[r] + play(A,l,r-1,nextPlayer )); //如果取最后一个数字

    	return score;
} 
```

#### 下一步:

- 我们首先会重新定义 **``play``** 函数，**``play``** 函数的返回值是**小六能拿的分数**。如果最终这个分数在总分数一半以上的话，小六能获胜，反之，小丁获胜或平手。
- 如果我是**小六** ，对于返回来的分数选择中，有两个选择。身为**小六**，如果他想要拿到最多的分，那他是不是得选择**最大**的那一个？
- 如果我是**小丁**，同样有两个选择。**_但_** 跟小六不一样的是，**_小丁会选择返回一个最小的数给小六因为她不想让小六获胜_** <br/> **提醒**：**``play``** 函数返回的是**小六**的分数_
 


最终我们的代码将能用 **``dp[l][r][player]``** 去表达我们的压缩状态，**``[l,r]``** 就如之前所说，表示当前剩下的数字的范围，**player** 代表当前是谁的回合，
我们用 **`0`** 代表**小六**，**`1`** 代表**小丁**。

#### 完整代码 1 </br>
>**时间**: **_O(n^2)_** <br/>
>**空间**: **_O(n^2)_** <br/>

 ```java
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
        if(l > r){                  // 数组为空
            return 0;  
        }  
        
        if(dp[l][r][player]!=0){   // 如果是已经访问过的状态，我们不需要再重新计算  
            return dp[l][r][player];  
        }  
  
        int nextPlayer = (player + 1) % 2;   // 下一个player是谁？我们用0代表小六，1代表小丁 
        int score = 0;  
  
        if(player == 0){  /***** 小六：对于返回的分数，我要取最大的那一个 ******/
            score=Math.max(score,A[l]+play(A,l+1,r,nextPlayer));  
            score=Math.max(score,A[r]+play(A,l,r-1,nextPlayer));  
        }  
        else{   /******小丁：hmm，为了不让小六赢，我得返回一个最小的数字给小六 *****/
            score=Math.min(play(A,l+1,r,nextPlayer),play(A,l,r-1,nextPlayer));  
        }  
  
        dp[l][r][player] = score; // 对状态的值的储存
        return score;  
      }  
}
```

---

以上是 ``TopDown`` 的 DP 方式，非常的好实现，并且从时间空间复杂度来说与 ``BottomUp`` 并没有什么区别。但是它的问题是因为用的是递归，所以会对 ``runtime stack`` 有很大的要求。我们再来看``BottomUp`` 是怎么写的，我们可以透过 ``TopDown`` 已经写好了的关系来进行一点简单的改变就行了。

**我们已知：**
 >- ``dp[l][r][0]=Math.max(A[l]+dp[l+1][r][1],A[r]+dp[l][r-1][1])``<br/>
 >- ``dp[l][r][1]=Math.min(dp[l+1][r][0],dp[l][r-1][0])``<br/>
 
**下一步：**
 - 我们需要注意的只剩下 dp 填表的顺序。我们需要保证求当前的状态时，之前的状态已经是记录好了的<br/>
 - 我们会按 ``(0,0),(1,1),(2,2)  ... (n,n), (0,1),(1,2),(2,3) ... (0,2),(1,3),(2,4)...`` 这样的顺序打表。<br/>


#### 完整代码 2
```java
class Solution {  
    public boolean stoneGame(int[] A) {  
        int dp[][][] = new int[A.length][A.length][2];  
        int sum = 0;  
		for (int i = 0; i < A.length; i++) {  
		     sum += A[i];  
		}  
  
        for (int i = 0; i < dp.length; i++) {  
            dp[i][i][0] = A[i];  
            dp[i][i][0] = 0;      // 对 base 的初始化
        }  
  
        for (int i = 1; i < dp.length; i++) {  
            for (int l = 0; l + i < dp.length; l++) {  
                dp[l][l + i][0] = Math.max(A[l] + dp[l + 1][l + i][1], A[l + i] + dp[l][l + i - 1][1]);  
                dp[l][l + i][1] = Math.min(dp[l + 1][l + i][0], dp[l][l + i - 1][0]);  
             }  
        }  
        
	/* dp[0][A.length-1][0] 代表小六能从数组区间[0,len(A)-1]能拿到的最大分数 */
        return dp[0][A.length - 1][0] * 2 > sum;  
     }  
}
```

---
---


## <h2 align="center"><b> Stone Game III </b><br><h2>

学会了**石子游戏 I** 之后，让我们来乘胜追击，一起来攻破一下难度是 **HARD** 的**石子游戏 III**！(为了方便读者，这次会使用 **C++**  进行实现)<br/>

>题意：给你一组数组 **``A=[1,2,3,7]``** 同样还是小六(先手)和小丁轮流从里面取数字，**_但_** ，两人这次只能从左边拿数字，一次可以拿 **1 ~ 3** 个，
>取完之后这数字会从 **``A``** 中移除，**问谁是最后的赢家如果两个人每次都采取对自己最有利的方案。分数最多者为胜利者**。>br/>
>从例子来看，无论小六怎么拿，**小丁都能获胜，因为她能拿到最后一个 ``7`` **


#### 分析： 
 - 我们可以像**石子游戏 I** 一样定义同样的 **``play``** 函数，返回的值是**小六**得到的分数。**如果最终小六的分数大于一半，小六获胜，反之平手或者小丁获胜**
 - 从**策略**来说，**石子游戏III** 与**石子游戏 I** 是一样的。每个 _player_ 都有 **2 种选择**。
 	- **小六**想拿一个使他总分更大的选择，**小丁**反之想返回给**小六**一个使他分数更小的选择
 - 通过观察，我们每次移除的数字都是从左边开始，我们可以用一个 **``l``** _(index)_  去表示当前还剩下那些数字。剩下的数字是 **``A[l : len(A)-1]``** 。 
   此外还要用一个 **``player``** 去表达当前的回合。(我们很好的压缩了状态)

#### 完整代码 1
```c++
class Solution {
    public:
    string stoneGameIII(vector<int>& A) {
        vector<vector<int>> dp(A.size(), vector<int>(2, INT_MIN));
        int sum = 0;

        for (int x : A) {
            sum += x;
        }

        int maxScore = dfs(dp, A, 0, 0);  // 小六能拿到最大的分数
		
	/*** 根据分数有三种情况 ******/
	
        if (maxScore * 2 > sum) {
            return "Alice";
        } else if (maxScore * 2 < sum) {
            return "Bob";
        } else {
            return "Tie";
        }
    }

    int dfs(vector<vector<int>>& dp, vector<int>& A, int l, int player) {
        if (l >= A.size()) {            // 所有数被取完的情况
            return 0;
        }

        if (dp[l][player] != INT_MIN) {    // 如果是已经访问过的状态，我们不需要再重新计算  
            return dp[l][player];
        }

        int nextPlayer = (player + 1) % 2;
        int res = INT_MIN;

        if (player == 0) {
            int score = 0;
            for (int i = 1; i <= 3; i++) {
                if (l + i - 1 < A.size()) {
			score += A[l + i - 1];  // 注意有可能剩下不够3个的情况，防止 outbound
		}
                res = max(res, score + dfs(dp, A, l + i, nextPlayer));
            }
        } else {
            res = INT_MAX;                    // 我们要取最小的关系，所以把res设成INT_MAX
            for (int i = 1; i <= 3; i++) {
                res = min(res, dfs(dp, A, l + i, nextPlayer));
            }
        }

        dp[l][player] = res;              // 对状态的值的储存
        return res;
    }
};
```
<br/>

___

**接下来是我们的 ``BottomUp``了:**

 - 首先我们的状态是以**``dp[l][player]``** 表示的，**``l``** 表示当前所剩下的石头的最左边(别忘了我们只能从左边开始取)，**``player``** 表示当前是谁
 - 我们只需要将上面递归的转换一下就可以了


#### 完整代码 2
```c++
class Solution {
    public:
    string stoneGameIII(vector<int>& A) {
        vector<vector<int>> dp(A.size(), vector<int>(2, INT_MIN));
        int sum = 0;
        for (int x : A) {
            sum += x;
        }

        for (int l = A.size() - 1; l >= 0; l--) {
            int mn = INT_MAX;
            int mx = INT_MIN;
            int score = 0;
            for (int j = 0; j < 3; j++) {       // player0 的三种选择
                if (l + j < A.size()) {         // 注意outbound
                    score += A[l + j];
                }
                if (l + j + 1 < A.size()) {
                    mx = max(mx, score + dp[l + j + 1][1]);
                } else {
                    mx = max(mx, score);
                }
            }

            for (int j = 0; j < 3; j++) {        // player0 的三种选择
                if (l + j + 1 < A.size()) {
                    mn = min(mn, dp[l + j + 1][0]);
                } else {
                    mn = min(mn, 0);
                }
            }
            dp[l][0] = mx;
            dp[l][1] = mn;
        }

        int maxScore = dp[0][0];

        if (maxScore * 2 > sum) {
            return "Alice";
        } else if (maxScore * 2 < sum) {
            return "Bob";
        } else {
            return "Tie";
        }
    }
};
```
