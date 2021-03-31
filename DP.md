
# <h1 align="center"><b>石子游戏系列:</b><br><h1>
	
## 主题：
今天我们将学习 Leetcode 里的 **Stone Game 系列**。 我们的目的是将通过学习这一系列的问题使我们对 **``Dynamic Programming``**，也就是我们熟悉的 **``DP(动态规划)``** 有一个更好的了解！<br/><br/>

#### :question: 动态规划是什么 (DP)？
 - 它是一种算法上的优化
 - 它是一种状态的压缩
 - 它是``cache``， 用来避免重复的计算
 <br/>

**:star:我们将会学习Strone Game 1 和 3 （Leetcode 877 和 Leetcode 1406 ），两者都是此系列的模板题目，通过学习这两题，希望能做到举1反3的效果！**
**:star: 同时我们还会学习TopDown 和BottomUp 的两种DP 解法。** 
- [Stone Game I](#stone-game-1)
	- [Stone Game I: Top Down](#stone-game-1-topdown) 
	- [Stone Game I: Bottom Up](#stone-game-1-bottomup) 
- [Stone Game III](#stone-game-3) 
	- [Stone Game III: Top Down](#stone-game-3-topdown) 
	- [Stone Game III: Bottom Up](#stone-game-3-bottomup) 



## 博弈入门:



在我们学习石子游系列之前，我们需要对**博弈**有简单的了解。 <br/>
这文章的目的并不是为了学习博弈论，而是通过简单的游戏系列来**学习 DP**，但是我们需要一些基础来开始。<br/><br/><br/>

1. **我们可以用我们熟悉的 ``猜拳``例子:** <br/>

 > 假设小六和小丁在玩猜拳，如果小六已经猜到了小丁会出布，那么小六肯定会出剪刀，因为剪刀能使小六获胜。
 > 但是，如果小丁已经猜到小六会猜到自己出布，_那么小丁会不会改变策略呢_ ？
 > 如果小六又猜到了小丁已经猜到了自己猜到了小丁的想法 (到这里已经开始有点乱了)，_他又会怎么做呢_ ？<br/>
 
  从以上例子我们可以看出，两个人的 **``猜``** 在不停的进行博弈，在这过程中都希望选出一个对自己最有利 (**``OPTIMAL``**) 的方案。 <br/><br/>

2. **最优(Optimal)方案** <br/>
什么是**最优的方案**呢？说的简单一点，就是当player 面前有多种选择的时候，他会选择对自己最有利的那一个。以以上的猜拳作为例子，对于小六来说，他有三种选择，那就是石头，剪子，和布啦！对于每一种选择，可能都会引起小丁不同的反应。 例如小六出布的话小丁猜到的概率可能更高，这种情况，小六就应该出剪刀或者石头，因为这两个没这么容易被猜到，对于自己会更加有利。<br/>**小提示**: 一般博弈类的题目都会有 Optimal 这字眼哦！一看到这字眼，我们就可以往这方面去想了

3. **穷举，贪心 ， DP**<br/>
穷举就是把所有的情况都列举出来的意思，也是暴力里面的最暴力。对于博弈类的问题，最简单的做法就是把所有的情况都列举出来然后选择最优的那个方案，但很多都是NP复杂度。但是也有很多博弈问题有贪心的解法(拥有所谓的**必胜策略**)。 而我们的石子游戏系列就是一种可以使用状态压缩从而避免NP复杂度的题目 (下面会详细说到什么是状态压缩)。


5. **小总**<br/>
博弈是一门博大精深的学问，我们并不会去深入探究。我们是试图通过简单的吧博弈 (石子游戏) 去更好的了解**DP**。

<br/><br/>





## TopDown 和 BottomUp : 
我们接下来还要了解一下什么是DP 的 TopDown 和 BottomUp，以及他们的区别是什么。对此，我们又请来了我们熟悉的朋友-**斐波那契数列**。<br/>
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
我们如果把n print一下的话，我们可以发现有很多个n 被重复访问了。因为**f(n)**的值是不变的，所以我们对于重复访问可以不去做重复的计算，而是把它的值在第一次访问完后给记录下来。<br/><br/>


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
我们可以看出**TopDown** 跟递归是非常相似的，我们只是加多了去记录重复的这一步。我们从**f(n)** 开始一层一层往下走，最终到达 **f(1)**，最后把最下面的值回传上来。

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
**BottomUp** 其实就是对关系式的表达。我们的关系式是**f(n)=f(n-1)+f(n-2)**。但与**TopDown**不同的是我们需要先initialize **Base Case**, 这里的**Base** 就是 n=1 和 n=2 的时候，也就是我们的first和second。 然后我们根据**Base** 一层一层的往上面建立，从**f(1)** 建立到**f(n)**。
<br/><br/>

**区分两者**<br/>
1. **TopDown**， **BottomUp** 两个听起来好像不太好记，但其实不然。一个是从上到下，一个是从下到上，这里我们只要记住 **Base Case** 是下就行了。做**TopDown**的时候，**Base Case**是最后被访问的，而**BottomUp**则相反，我们会先 initialize **Base Case**<br/>

2. 从时间和空间复杂度来说两者都是差不多的，但是因为**TopDown**用的是递归，对于**Runtime Stack**的要求会比较大<br/>

3. **BottomUp** 的关系都能从**TopDown**的递归那里看出来，第一眼看不出关系式的话，建议先从**TopDown**入手


<br/>
<h1 id="stone-game-1" align="center"><b> Stone Game I </b><h1>

### 题意：
>给你一组数组 **``A=[5,3,4,5]``** 小六(先手)和小丁轮流从里面取数字，**``但``**，两人只能拿第一个或者最后一个数字，取完之后这数字会从 **``A``** 中移除，问谁是最后的赢家如果两个人每次都采取对自己最有利的方案。**分数最多者为胜利者**。<br/>

<h3 id="stone-game-1-topdown" >:bulb: Approach 1: Top Down<h3>
	
###  思路：

 - 对于每个 _player_ 来说，他都有**两种选择** (以**第一轮**为例):
 	1. 如果小六选了 `A[0]` , 那么他会留下 **``[3,4,5]``**  给小丁做选择
 	2. 如果他选的是 `A[3]` , 他会留下 **``[5,3,4]``** 给小丁做选择<br/><br/>
 - 如果我们选择**暴力**的话，我们可以把所有的可能性都列举出来。当某一个 ``player`` 选掉一个数字后，他可以把剩下的数字递归给下一位 ``player``，当所有数字取完时，游戏结束<br/><br/>
 - 这里我们可以得到一个观察，因为数字只能取**第一个**或者**最后一个**，我们可以用一个区间 **``[l,r]``** 去表达剩下的数字。如果我们取第一个，新的 **``A``** 会变成 `[l+1,r]` 如果我们取最后一个，新的 **``A``** 是 `[l,r-1]` <br/>
 > 这就是我们所谓的**状态压缩**，我们用两个数字来表达数组的状态，这里的状态是指它还剩下哪些数字，而不是真的把数字从A里面移除掉，因为这样会导致很高的时间复杂度<br/>
 - 除此之外，我们还需要一个额外的 **``player``** (variable) 去记录当前是谁的回合 (不同的 ``player`` 会用不同的决策)
<br/>


**:heavy_exclamation_mark: 表达状态转移的代码。代码非完整，我们还需要对每个人采用的 ``决策 (博弈)`` 进行分析**
 
 ```java
 
/* 我们可以用以下来表达状态变化的简单的递归模式 */

public int play(int A[],int l,int r,int player){ 
	if (l == r) {                   // 如果只剩一个，直接取这一个
		return A[l]；
	}
	int nextPlayer = (player + 1) % 2;
	int score = 0;

	score = Math.max(score, A[l] + play(A,l+1,r,nextPlayer )); // 如果取第一个数字
	score = Math.max(score, A[r] + play(A,l,r-1,nextPlayer )); // 如果取最后一个数字
	return score;
} 
```

#### 分析:

- 我们首先会重新定义 **``play``** 函数，**``play``** 函数的返回值是**小六能拿的分数**。如果最终这个分数在总分数一半以上的话，小六能获胜，反之，小丁获胜或平手<br/><br/>
- 如果我是**小六** ，对于返回来的分数选择中，有两个选择。身为**小六**，如果他想要拿到最多的分，他得选择**最大**的那一个<br/><br/>
- 如果我是**小丁**，同样有两个选择。**``但``** 跟小六不一样的是，**小丁会选择返回一个最小的数给小六因为她不想让小六获胜** <br/><br/>
- 最终我们的代码将能用 **``dp[l][r][player]``** 去表达我们的压缩状态，**``[l,r]``** 就如之前所说，表示当前剩下的数字的范围，**player** 代表当前是谁的回合<br/><br/>
- 我们用 **`0`** 代表**小六**，**`1`** 代表**小丁** <br/><br/>





### :triangular_flag_on_post: 完整代码 ``Top Down``

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
        if(l == r){// 数组为1
            return A[l];  
        }  
        
        if(dp[l][r][player]!=0){   // 如果是已经访问过的状态，我们不需要再重新计算  
            return dp[l][r][player];  
        }  
  
        int nextPlayer = (player + 1) % 2;   // 下一个player是谁？我们用0代表小六，1代表小丁 
        int score = 0;  
	  
		   //我们可以从递归这里推导出关系式，从而转换成BottomUp
        if(player == 0){ // 小六：对于返回的分数，我要取最大的那一个 
            score=Math.max(score,A[l]+play(A,l+1,r,nextPlayer));  
            score=Math.max(score,A[r]+play(A,l,r-1,nextPlayer));  
        }  
        else{ //小丁：hmm，为了不让小六赢，我得返回一个最小的数字给小六
              //这里不需要加A[l] 或者A[r]，因为play 函数是小六的分数
            score=Math.min(play(A,l+1,r,nextPlayer),play(A,l,r-1,nextPlayer));  
        }  
  
        dp[l][r][player] = score; // 对状态的值的储存
        return score;  
      }  
}
```
**代码总结**

 - **play** 函数就是我们的主要逻辑所在，**play**函数值是小六能得到的最大分数<br><br>
 -  我们用 **[l,r]** 去表示还剩下哪些数字，**player**去表示当前的回合。**nextPlayer**是下一个回合。如果当前回合是0，下一个回合就是1。如果当前回合是1，下一个回合就是0。我们用 **nextPlayer = (player + 1) % 2**<br><br>
 -  我们有两种情况，**player**是0和是1的情况。如果是0 (先手小六)，他想得到的是**最大**的分数，所以我们传回一个max。如果是1（小丁后手），她想要小六拿到**最小**，所以我们回传一个最小。<br/>**注意**，小丁的回合我们不用加上A[l] 或者A[r]，因为**play**是小六的分数，所以小丁回合的A[l] 或者 A[r] 不用归入小六里面<br/><br/>
- 空间复杂度就非常明显是 **2 O(n^2)** ，时间复杂度是play函数的状态个数乘每个play函数的时间复杂度。我们一共有**2n^2**的状态，play函数的空间是O(1)，所以时间复杂度也是**2 O(n^2)**。 简化一下就是**O(n^2)**



<br/>
 <h3 id="stone-game-1-bottomup" >:bulb: Approach 2: Bottom Up<h3>

###  思路：
 - 与**TopDown** 不同的是，我们会先 initialize **Base Case**，然后通过**Base Case** 一层一层的根据关系式往上面建立<br/><br/>
 - 在这里我们的 **Base Case** 是 **l==r** 的时候，**l==r**就是只剩下一个石子的时候，如果这个回合是小六，他能得到所有的分数，如果是小丁，她得到的是0分（还记得**play**函数是指小六的分数而不是小丁的吗）
 


#### DP 关系转移:
```
小六：
我们用dp[l][r][0] 去表示当前是 小六的回合，他能拿的数组[l:r]的数

1. 如果小六拿第一个，区间会变成[l+1:r],下一个player是1，那就是A[l]+dp[l+1][r][1]
2. 如果小六拿最后一个，区间会变成[l:r-1],下一个player是1，那就是A[r]+dp[l][r-1][1]
3. 小六要取最大的那一个，综上，关系式就是 dp[l][r][0]=Math.max(A[l]+dp[l+1][r][1],A[r]+dp[l][r-1][1])
```

<br/>

```
小丁：
我们用dp[l][r][1] 去表示当前是 小丁的回合，她能拿的数组[l:r]的数。(小丁的分数不计入play函数里面)

1. 如果小丁拿第一个，区间会变成[l+1:r],下一个player是0，那就是dp[l+1][r][0]
2. 如果小丁拿最后一个，区间会变成[l:r-1],下一个player是0，那就是dp[l][r-1][0]
3. 小丁要取最小的那一个，综上，关系式就是 dp[l][r][1]=Math.min(dp[l+1][r][0],dp[l][r-1][0])

```
 
**下一步：**
 - 我们需要注意的只剩下 dp 填表的顺序。**我们需要保证求当前的状态时，之前的状态已经是记录好了的**<br/><br/>
 - 我们会按 ``(0,0),(1,1),(2,2)  ... (n,n), (0,1),(1,2),(2,3) ... (0,2),(1,3),(2,4)...`` 这样的顺序打表<br/>


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
            dp[i][i][0] = 0; // 对 base 的初始化
        }  
  
        for (int i = 1; i < dp.length; i++) {  
            for (int l = 0; l + i < dp.length; l++) {  
                dp[l][l + i][0] = Math.max(A[l] + dp[l + 1][l + i][1], A[l + i] + dp[l][l + i - 1][1]);  
                dp[l][l + i][1] = Math.min(dp[l + 1][l + i][0], dp[l][l + i - 1][0]);  
             }  
        }  
        
	    //dp[0][A.length-1][0] 代表小六能从数组区间[0,len(A)-1]能拿到的最大分数 
        return dp[0][A.length - 1][0] * 2 > sum;  
     }  
}
```

---
**代码总结**

 - 对于**BottomUP** 我们先对**Base Cass** 进行初始化，这里的**Base Case**是 **l==r**的时候<br/><br/>
 - 这里的关系式我们可以从 **TopDown** 那里看的出来，做DP的时候建议先**TopDown**再**BottomUP**会比较容易<br/><br/>
 - 需要注意打表的顺序，保证 **求当前的状态时，之前的状态已经是记录好了的**.<br/><br/>
 - 时间和空间复杂度跟**TopDown** 是一样的


<br/><br/>
<h1 id="stone-game-3" align="center"><b> Stone Game III </b></h1>

学会了**石子游戏 I** 之后，让我们来乘胜追击，一起来攻破一下难度是 **HARD** 的**石子游戏 III**！(为了方便读者，这次会使用 **C++**  进行实现) <br/>

### 题意：
>给你一组数组 **``A=[1,2,3,7]``** 同样还是小六(先手)和小丁轮流从里面取数字，**_但_** ，两人这次只能从左边拿数字，一次可以拿 **1~3** 个，取完之后这数字会从 **``A``** 中移除，**问谁是最后的赢家如果两个人每次都采取对自己最有利的方案。分数最多者为胜利者**。<br/>


<h3  id="stone-game-3-topdown">:bulb: Approach 1: Top Down<h3>

###  思路：

 - 对于每个 _player_ 来说，他都有**三种选择** (以**第一轮**为例):
 	1. 如果小六选了 `A[0]` , 那么他会留下 **``[2,3,7]``**  给小丁做选择
 	2.  如果他选的是 `A[0]+A[1]` , 他会留下 **``[3,7]``** 给小丁做选择<br/>
 	3. 如果他选的是 `A[0]+A[1]+A[2]` , 他会留下 **``[7]``** 给小丁做选择<br/>
>从例子来看，无论小六怎么拿，**小丁都能获胜，因为她能拿到最后一个 ``7``。**

 - 如果我们选择**暴力**的话，我们可以把所有的可能性都列举出来。当某一个 ``player`` 选掉一个数字后，他可以把剩下的数字递归给下一位 ``player``，当所有数字取完时，游戏结束<br/><br/>
 - 这里我们可以得到一个观察，因为数字只能从左边取，我们可以用一个区间 **``[l,r]``** 去表达剩下的数字。如果我们取第一个，新的 **``A``** 会变成 `[l+1,r]` 如果我们取前两个，新的 **``A``** 是  `[l+2,r]`。并且因为 r 是固定的，我们可以把其省略 <br/><br/>
 - 同 **Stone Game I** 同理，我们还需要一个额外的 **``player``** (variable) 去记录当前是谁的回合 (不同的 ``player`` 会用不同的决策)
<br/> 

#### 分析:

- **``play``** 函数的返回值是**小六能拿的分数**。如果最终这个分数在总分数一半以上的话，小六能获胜，反之，小丁获胜或平手<br/><br/>
- 身为**小六**，如果他想要拿到最多的分，选择**最大**的那一个<br/><br/>
- 身为**小丁**，同样有三个选择。**``但``** 跟小六不一样的是，**小丁会选择返回一个最小的数给小六因为她不想让小六获胜** <br/><br/>
- 最终我们的代码将能用 **``dp[l][player]``** 去表达我们的压缩状态，**``[l]``** 就如之前所说，表示当前剩下的数字的范围，**player** 代表当前是谁的回合<br/><br/>
- 我们用 **`0`** 代表**小六**，**`1`** 代表**小丁** <br/><br/>





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
        } 
        else {
            res = INT_MAX; // 我们要取最小的关系，所以把res设成INT_MAX
            for (int i = 1; i <= 3; i++) { //这里不用在意outbound，outbound情况下递归直接返回0
                res = min(res, dfs(dp, A, l + i, nextPlayer));
            }
        }

        dp[l][player] = res;// 对状态的值的储存
        return res;
    }
};
```
<br/>

**代码总结**
 - 同**Strong Game I** 一样，**play** 函数就是我们的主要逻辑所在，**play**函数值是小六能得到的最大分数<br><br>
 -  我们用 **[l,r]** 去表示还剩下哪些数字，**player**去表示当前的回合。**nextPlayer**是下一个回合。如果当前回合是0，下一个回合就是1。如果当前回合是1，下一个回合就是0。我们用 **nextPlayer = (player + 1) % 2** 但因为 **r**是固定的，我们可以单纯用 **l**来表示就够了<br><br>
 -  我们有三种情况，**player**是0和是1的情况。如果是0 (先手小六)，他想得到的是**最大**的分数，所以我们传回一个max。如果是1（小丁后手），她想要小六拿到**最小**，所以我们回传一个最小。<br/>**注意**，小丁的回合我们不用加上A[l] 或者A[r]，因为**play**是小六的分数，所以小丁回合的A[l] 或者 A[r] 不用归入小六里面<br/><br/>
- 空间复杂度就非常明显是 **2 O(n)** ，时间复杂度是play函数的状态个数乘每个play函数的时间复杂度。我们一共有**2n**的状态，play函数的空间是O(3)，所以时间复杂度也是**3 O(2n)**。 简化一下就是**O(n)**

<br/><br/>
<h3 id="stone-game-3-bottomup">:bulb: Approach 2: Bottom Up <h3>

###  思路：
 - 同理，我们要先对 **Base Case** 进行 initialize。这里的**Base Case** 是都取完outbound的时候，所以可以不用进行特别的初始化 <br/><br/>
 
#### DP 关系转移:
```
小六：
我们用dp[l][0] 去表示当前是 小六的回合，他能拿的数组[l:A.size()-1]的数

1. 如果小六拿第一个，区间会变成[l+1],下一个player是1，那就是A[l]+dp[l+1][1]
2. 如果小六拿第前两个，区间会变成[l+2],下一个player是1，那就是A[l]+A[l+1]+dp[l+2][1]
3. 如果小六拿第前三个，区间会变成[l+3],下一个player是1，那就是A[l]+A[l+1]+A[l+2]+dp[l+3][1]
4. 综上，关系式就是dp[l][0]=max(A[l]+dp[l+1][1],A[l]+A[l+1]+dp[l+2][1],A[l]+A[l+1]+A[l+2]+dp[l+3][1]) 可能会有outbound的情况，我们可以用if statement做特殊处理
```

<br/>

```
小丁：
我们用dp[l][1] 去表示当前是 小丁的回合，她能拿的数组[l:A.size()-1]的数。(小丁的分数不计入play函数里面)

1. 如果小丁拿第一个，区间会变成[l+1],下一个player是1，那就是A[l]+dp[l+1][1]
2. 如果小丁拿第前两个，区间会变成[l+2],下一个player是1，那就是A[l]+A[l+1]+dp[l+2][1]
3. 如果小丁拿第前三个，区间会变成[l+3],下一个player是1，那就是A[l]+A[l+1]+A[l+2]+dp[l+3][1]
4. 综上，关系式就是dp[l][1]=min(dp[l+1][0],dp[l+2][0],dp[l+3][0]) 可能会有outbound的情况，这种情况dp[outbound][0]=0

```

**下一步：**
 - 我们需要注意的只剩下 dp 填表的顺序。**我们需要保证求当前的状态时，之前的状态已经是记录好了的**<br/><br/>
 - 从关系式 **dp[l][0]=max(A[l]+dp[l+1][1],A[l]+A[l+1]+dp[l+2][1],A[l]+A[l+1]+A[l+2]+dp[l+3][1])** 和**dp[l][0]=min(dp[l+1][1],dp[l+2][0],dp[l+3][0])** 和 我们这里可以直接按直线的顺序来打表，从 n 开始到 0 进行打表

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
		
		//三种情况
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

**代码总结**
	

 - 和**Strone Game** 一样非常类似，如果我们一开始不知道怎么推理关系式，我们可以先从**TopDown** 或者暴力开始 <br/><br/>
 - 知道关系之后我们就可以打表了，先初始化**Base Case**， 然后**只要保证求当前的状态时，之前的状态已经是记录好了的就没问题**<br/><br/>


<br/><br/>
<h1 id="stone-game-1" align="center"><b> 总结</b><h1>

LeetCode 的 **Stone Game** 系列有七题之多，但大多数的套路都是差不多的，我们通过学习以上的两题希望能对 **博弈** 和 **DP** 有一个更好的了解。<br/><br/>
对于**DP**，它是一种状态的表达和压缩。由**石子游戏** 我们可以看见，从暴力出发我们是把数字从数组里移除掉再把这个新的数组递给下一个回合。但是因为其移取的规律性，我们可以用区间
**[l:r]** 来进行对数组状态的表达，从而可以做到像**斐波那契** 那样进行cache 和记忆化。DP题目我们一般都可以从暴力或者**TopDown** 出发，找出关系后可进行优化成**BottomUp**的模式<br/><br/>

对于博弈，它是一门比较博大精深的东西。这里我们涉及到的只是比较基本的层面，加上**Stone Game** 取石子是有规律性的所以我们可以做到一个压缩达到优化。也有一些游戏系列是有必胜策略的，（听说五子棋先手必胜）这种题目我们可以用贪心来解，希望下次有机会分享分享这一类的题目。也有一些题目是NP 复杂度的，但我们今天这文章主要目的是通过简单的博弈学习**DP**, 希望有用
