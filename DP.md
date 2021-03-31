
# <h1 align="center"><b>石子游戏系列:</b><br><h1>
	
## 主题：
今天我们将学习 Leetcode 里的 **Stone Game 系列**。 我们的目的是将通过学习这一系列的问题使我们对 **``Dynamic Programming``**，也就是我们熟悉的 **``DP(动态规划)``** 有一个更好的了解！<br/><br/>

**:star:我们将会学习Strone Game 1 和 3，两者都是此系列的模板题目，通过学习这两题，希望能做到举1反3的效果！**
**:star: 同时我们还会学习TopDown 和BottomUp 的两种DP 解法。** 
- [Stone Game I](#stone-game-1)
	- [Stone Game I: Top Down](#stone-game-1-topdown) 
	- [Stone Game I: Bottom Up](#stone-game-1-bottomup) 
- [Stone Game III](#stone-game-3) 
	- [Stone Game III: Top Down](#stone-game-3-topdown) 
	- [Stone Game III: Bottom Up](#stone-game-3-bottomup) 

#### :question: 动态规划是什么 (DP)？
 - 它是一种算法上的优化
 - 它是一种状态的压缩
 - 它是``cache``， 用来避免重复的计算
 <br/>

## 博弈入门:



在我们学习石子游系列之前，我们需要对**博弈**有简单的了解。 <br/>
这文章的目的并不是为了学习博弈论，而是通过简单的游戏系列来**学习 DP**，但是我们需要一些基础来开始。<br/><br/><br/>

1. **我们可以用我们熟悉的 ``猜拳``例子:** <br/>

 > 假设小六和小丁在玩猜拳，如果小六已经猜到了小丁会出布，那么小六肯定会出剪刀，因为剪刀能使小六获胜。
 > 但是，如果小丁已经猜到小六会猜到自己出布，_那么小丁会不会改变策略呢_ ？
 > 如果小六又猜到了小丁已经猜到了自己猜到了小丁的想法 (到这里已经开始有点乱了)，_他又会怎么做呢_ ？<br/>
 
  从以上例子我们可以看出，两个人的 **``猜``** 在不停的进行博弈，在这过程中都希望选出一个对自己最有利 (**``OPTIMAL``**) 的方案。 <br/><br/>

2. **最优(Optimal)方案** <br/>
什么是**最优的方案**呢？说的简单一点，就是当player 面前有多种选择的时候，他会选择对自己最有利的那一个。以以上的猜拳作为例子，对于小六来说，他有三种选择，那就是石头，剪子，和布啦！对于每一种选择，可能都会引起小丁不同的反应。 例如小六出布的话小丁猜到的概率可能更高，这种情况，小六就应该出剪刀或者石头，因为这两个没这么容易被猜到，对于自己会更加有利。<br/>**小提示**: 一般博弈类的题目都会有 Optimal 这字眼哦！

3. **穷就，贪心 ， DP**<br/>
穷举就是把所有的情况都列举出来的意思，也是暴力里面的最暴力。对于博弈类的问题，最简单的做法就是把所有的情况都列举出来然后选择最优的那个方案，但很多都是NP复杂度。但是也有很多博弈问题有贪心的解法(拥有所谓的**必胜策略**)。 而我们的石子游戏系列就是一种可以使用状态压缩从而避免NP复杂度的题目 (下面会详细说到什么是状态压缩)。


5. **小总**<br/>
博弈是一门博大精深的学问，我们并不会去深入探究。我们是试图通过简单的吧博弈 (石子游戏) 去更好的了解**DP**。


---




## TopDown 和 BottomUp : 
我们接下来还要了解一下什么是DP 的 TopDown 和 BottomUp，以及他们的区别是什么。对此，我们又请来了我们熟悉的朋友-斐波那契数列。<br/>
我们已知斐波那契数列的关系式 ：**f(n)=f(n-1)+f(n-2)**

 1. 下面是我们使用**递归**的暴力代码

 ```java
    public int f(int n) {
        if(n <= 2){
            return n;
        }
        return f(n - 2) + f(n - 1);
    }
```
我们如果把n print一下的话，我们可以发现有很多个n 被重复访问了。因为**f(n)**的值是不变的，所以我们对于重复访问可以不去做重复的计算，而是把它的值在第一次访问完后给记录下来<br/><br/>


2. 从**递归**转换到 **TopDown**
 ```java
   class Solution {
   
    int dp[]=new int[100]; //假设 n 在100以内
    
    public int f(int n) {
        if(n <= 2){//base case
            return n;
        }
        
        if(dp[n] != 0 ){//非第一次访问，值已被记录，可直接返回
            return dp[n];
        }
        
        dp[n] = f(n - 2) + f(n - 1); //第一次访问并记录f(n)的值
        return dp[n];
    }
}
```
我们可以看出**TopDown** 跟递归是非常相似的，我们只是加多了去记录重复的这一步

<br/><br/>
3. **BottomUp**
 ```java
public int climbStairs(int n) {
        if(n <= 2){
            return n;
        }
        //对basecase 的初始化
        int first = 1;
        int second = 2;
        
        for(int i = 2; i < n; i++){
            int temp = second;
            second = first + second;
            first = temp;
        }
        return second;
    }
```
从时间负责度和空间负责度来说**TopDown**和 **BottomUp** 并没有太大区别。但是**TopDown**需要用到递归的关系，对 **Runtime Stack**的要求比较高。

<br/><br/>
**如何区分两者**<br/>
**TopDown**， **BottomUp** 两个听起来好像不太好记，但其实不然。一个是从上到下，一个是从下到上，这里我们只要记住 **Base Case** 是下就行了。做**TopDown**的时候，**Base Case**是最后被访问的。而**BottomUp**则相反，我们会先initialize **Base Case**



<br/>
<h1 id="stone-game-1" align="center"><b> Stone Game I </b><h1>

### 题意：
>给你一组数组 **``A=[5,3,4,5]``** 小六(先手)和小丁轮流从里面取数字，**``但``**，两人只能拿第一个或者最后一个数字，
>取完之后这数字会从 **``A``** 中移除，问谁是最后的赢家如果两个人每次都采取对自己最有利的方案。**分数最多者为胜利者**。<br/>

<h3 id="stone-game-1-topdown" >:bulb: Approach 1: Top Down<h3>
	
###  分析：

 - 对于每个 _player_ 来说，他都有**两种选择** (以**第一轮**为例):
 	1. 如果小六选了 `A[0]` , 那么他会留下 **``[3,4,5]``**  给小丁做选择
 	2. 如果他选的是 `A[3]` , 他会留下 **``[5,3,4]``** 给小丁做选择。
 - 如果我们选择**暴力**的话，我们可以把所有的可能性都列举出来。当某一个 ``player`` 选掉一个数字后，他可以把剩下的数字递归给下一位 ``player``，当所有数字取完时，游戏结束
 - 这里我们可以得到一个观察，因为数字**只能**取**第一个**或者**最后一个**，我们可以用一个区间 **``[l,r]``** 去表达剩下的数字。如果我们取第一个，新的 **``A``** 会变成 `[l+1,r]` 
   如果我们取最后一个，新的 **``A``** 是 `[l,r-1]`  (这就是我们所谓的**状态压缩**，用两个数字来表达数组的状态，这里的状态是指它还剩下哪些数字)
 - 除此之外，我们还需要一个额外的 **``player``** (variable) 去记录当前是谁的回合 (不同的 ``player`` 会用不同的决策)
<br/>


**:heavy_exclamation_mark: 下面并不是完结的代码。我们还需要对每个人采用的 ``决策 (博弈)`` 进行分析**
 
 ```java
 
/* 我们可以用以下来表达状态变化的简单的递归模式 */

public int play(int A[],int l,int r,int player){ 
	if (l > r) {                   // 如果都取完了的情况 
		return 0；
	}
	int nextPlayer = (player + 1) % 2;
	int score = 0;

	score = Math.max(score, A[l] + play(A,l+1,r,nextPlayer )); // 如果取第一个数字
	score = Math.max(score, A[r] + play(A,l,r-1,nextPlayer )); // 如果取最后一个数字
	return score;
} 
```

#### 下一步:

- 我们首先会重新定义 **``play``** 函数，**``play``** 函数的返回值是**小六能拿的分数**。如果最终这个分数在总分数一半以上的话，小六能获胜，反之，小丁获胜或平手。
- 如果我是**小六** ，对于返回来的分数选择中，有两个选择。身为**小六**，如果他想要拿到最多的分，那他是不是得选择**最大**的那一个？
- 如果我是**小丁**，同样有两个选择。**``但``** 跟小六不一样的是，**小丁会选择返回一个最小的数给小六因为她不想让小六获胜** <br/> **:bangbang: 提醒：** **``play``** 函数返回的是**小六**的分数
- 最终我们的代码将能用 **``dp[l][r][player]``** 去表达我们的压缩状态，**``[l,r]``** 就如之前所说，表示当前剩下的数字的范围，**player** 代表当前是谁的回合，
我们用 **`0`** 代表**小六**，**`1`** 代表**小丁**。

### :triangular_flag_on_post: 完整代码 ``Top Down``
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
  
        if(player == 0){                      /* 小六：对于返回的分数，我要取最大的那一个 */
            score=Math.max(score,A[l]+play(A,l+1,r,nextPlayer));  
            score=Math.max(score,A[r]+play(A,l,r-1,nextPlayer));  
        }  
        else{                                /* 小丁：hmm，为了不让小六赢，我得返回一个最小的数字给小六 */
            score=Math.min(play(A,l+1,r,nextPlayer),play(A,l,r-1,nextPlayer));  
        }  
  
        dp[l][r][player] = score; // 对状态的值的储存
        return score;  
      }  
}
```


 <h3 id="stone-game-1-bottomup" >:bulb: Approach 2: Bottom Up<h3>

###  分析：
以上是 ``Top Down`` 的 DP 方式，非常的好实现，并且从时间空间复杂度来说与 ``Bottom Up`` 并没有什么区别。但是它的问题是因为用的是递归，所以会对 ``runtime stack`` 有很大的要求。我们再来看``BottomUp`` 是怎么写的，我们可以透过 ``Top Down`` 已经写好了的关系来进行一点简单的改变就行了。

**我们已知：**
 - ``dp[l][r][0]=Math.max(A[l]+dp[l+1][r][1],A[r]+dp[l][r-1][1])``<br/>
 - ``dp[l][r][1]=Math.min(dp[l+1][r][0],dp[l][r-1][0])``<br/>
 
**下一步：**
 - 我们需要注意的只剩下 dp 填表的顺序。我们需要保证求当前的状态时，之前的状态已经是记录好了的<br/>
 - 我们会按 ``(0,0),(1,1),(2,2)  ... (n,n), (0,1),(1,2),(2,3) ... (0,2),(1,3),(2,4)...`` 这样的顺序打表。<br/>


### :triangular_flag_on_post: 完整代码 ``Bottom Up``
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

<h1 id="stone-game-3" align="center"><b> Stone Game III </b></h1>

学会了**石子游戏 I** 之后，让我们来乘胜追击，一起来攻破一下难度是 **HARD** 的**石子游戏 III**！(为了方便读者，这次会使用 **C++**  进行实现) <br/>

### 题意：
>给你一组数组 **``A=[1,2,3,7]``** 同样还是小六(先手)和小丁轮流从里面取数字，**_但_** ，两人这次只能从左边拿数字，一次可以拿 **1~3** 个，
>取完之后这数字会从 **``A``** 中移除，**问谁是最后的赢家如果两个人每次都采取对自己最有利的方案。分数最多者为胜利者**。<br/>
>从例子来看，无论小六怎么拿，**小丁都能获胜，因为她能拿到最后一个 ``7``**

<h3  id="stone-game-3-topdown">:bulb: Approach 1: Top Down<h3>

### 分析： 
 - 我们可以像**石子游戏 I** 一样定义同样的 **``play``** 函数，返回的值是**小六**得到的分数。**如果最终小六的分数大于一半，小六获胜，反之平手或者小丁获胜**
 - 从**策略**来说，**石子游戏III** 与**石子游戏 I** 是一样的。每个 _player_ 都有 **2 种选择**。
 	- **小六**想拿一个使他总分更大的选择，**小丁**反之想返回给**小六**一个使他分数更小的选择
 - 通过观察，我们每次移除的数字都是从左边开始:
 	- 我们可以用一个 **``l``** (index)  去表示当前还剩下那些数字
   	- 剩下的数字是 **``A[l : len(A)-1]``**
   	- 此外还要用一个 **``player``** 去表达当前的回合。(我们很好的压缩了状态)

#### :triangular_flag_on_post: 完整代码 ``Top Down``
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


<h3 id="stone-game-3-bottomup">:bulb: Approach 2: Bottom Up <h3>

###  分析：
 - 首先我们的状态是以 **``dp[l][player]``** 表示的，**``l``** 表示当前所剩下的石头的最左边(别忘了我们只能从左边开始取)，**``player``** 表示当前是谁
 - 我们只需要将上面递归的转换一下就可以了


### :triangular_flag_on_post: 完整代码 ``Bottom Up``
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
