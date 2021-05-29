

# LeetCode Bi-Weekly Contest 53
这是对 LeetCode 第238场周赛的简单讲解和评论。以下会包括对**Leetcode双周赛 53**的每一题的题解，代码，难度分析，题目类型，以及一些个人对这题的看法等，希望读者们喜欢！<br/><br/>

**:bulb:注意** ： 题解并不一定都是最优解（代码以pass 了 LeetCode 的 Oline Judge 为准），文章初衷是给读者们提供一定的想法和问题的解决方案

 1876. [Substrings of Size Three with Distinct Characters](https://leetcode.com/problems/substrings-of-size-three-with-distinct-characters)<br/>
 1877. [Minimize Maximum Pair Sum in Array](https://leetcode.com/problems/minimize-maximum-pair-sum-in-array)<br/>
 1878. [Get Biggest Three Rhombus Sums in a Grid](https://leetcode.com/problems/get-biggest-three-rhombus-sums-in-a-grid)<br/>
 1879. [Minimum XOR Sum of Two Arrays](https://leetcode.com/problems/minimum-xor-sum-of-two-arrays)<br/>



## 前言 : 参加Contest 的好处

 1. 每周抽点时间去练习和学习新的题目这对于个人的编程能力与问题解决能力都有很大的帮助
 2. 在规定的时间内解决一定的题目，这和真实的**OA (Online Assessment)** 或者 **Onsite** 面试是一样的，可以把它当作一个很好的 Mock 练习机会
 3. 当你有碰到做不出的题目的时候，你能知道自己的弱项在哪里，然后可以根据这进行加强和练习，比赛是一面给自己的镜子
<br/><br/><br/>

## 对本场比赛的一些看法
本场比赛难度相对比较正常，都是比较常规的题目，第三题稍微有点难度需要一点思考<br/><br/>

 **1876**. 简单的暴力签到题<br/>
 **1877**. 一道贪心和双指针题目<br/>
 **1878**. 一道观察型的题目<br/>
 **1879**. 使用 bit 进行压缩的DP题目<br/><br/>



## 1876 Substrings of Size Three with Distinct Characters

#### 题意：
>给你一个字符串，对于这个字符串每个长度为3的子字符串，如果此子字符串没有重复的字母它就是好的，问有多少个好的子字符串？<br/><br/>
>例子：xyzzaz ： xyz，yzz，zza，zaz
>这里只有 xyz 是好的


### 思路：
1. 对于每个子字符串看看他们有没有重复的即可，为了方便可以直接使用Set，如果Set的size是3，它就是好的<br/><br/>


### :bulb:代码：
```
class Solution {
  public int countGoodSubstrings(String s) {
    int res = 0;
    for (int i = 0; i < s.length() - 2; i++) {
      Set < Character > set = new HashSet < > ();
      //以 i 作为子字符串的开头，子字符串是 S[i,i+2]
      set.add(s.charAt(i));
      set.add(s.charAt(i + 1));
      set.add(s.charAt(i + 2));
      if (set.size() == 3) res++;
    }
    return res;
  }
}
```

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(n) 
  - 空间复杂度：O(1)
<br/><br/>



## 1877 Minimize Maximum Pair Sum in Array
#### 题意：
>给一个数组，它有2N个数字。现在把这个数组分成N组，每组两个数字。我们要最小化每组数字的和里最大的数<br/><br/>
>听起来绕口，直接上例子。A = [3,5,2,3] 
>分成两pair，（3 ，3），（5，2）， answer = max(6,7) = 7



### :bulb:代码：

### 思路：

 - 这题我们需要考虑的是如何分配数字<br/><br/>
 - 把数组排序，使最大的数字和最小的数字进行匹配，然后在这些 pair 里找出最大的那个就是答案<br/><br/>
 **证明**：
 - 假设我们现在的数组是排好序的，我们现在是要找所有的pair 里sum 最大的那个，并且我们要使其越小越好<br/><br/>
 - 我们的想法是把 A[i] 和 A[n-i-1] 进行匹配，我们以第一对 A[0] 和 A[n-1] 作为例子。如果和A[n-1] 匹配的不是A[0] 而是其它的数字 （因为是排好序的关系其它的数字肯定大于A[0]），那么ans 比起原先的 ans >=A[0]+A[n-1] 就变成了 ans >=A[i !=0] + A[n-1]，这显然不是一个好的选择

### :bulb:代码：
```
class Solution {
  public int minPairSum(int[] A) {
    Arrays.sort(A);
    int l = 0, r = A.length - 1;
    int res = 0;
    while (l < r) {
      int sum = A[l++] + A[r--];
      res = Math.max(res, sum);
    }
    return res;
  }
}
```
#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(nlogn)，排序
  - 空间复杂度：O(1) 
<br/><br/>


## 1878 Get Biggest Three Rhombus Sums in a Grid
#### 题意：
>此题强烈建议去看原题，原题里的图给出了很好的解释。
>
>给你一个二维数组，找出所有的四角形和里最大的三个 unique，如果没有三个，则返回全部。
>什么是四角形？！
>
>0 1 0  <br/>
>1 0 1 <br/>
>0 1 0
>
>0 0 1 0 0<br/>
>0 1 0 1 0<br/>
>1 0 0 0.1<br/>
>0 1 0 1 0<br/>
>0 0 1 0 0<br/>
>
>为了方便以下的思路解释，我们称以下的斜线分别为左斜线和右斜线<br/>
> 0 0 1<br/>
> 0 1 0  （左斜线）<br/>
> 1 0 0<br/>
>  
> 1 0 0<br/>
> 0 1 0 （右斜线）<br/>
> 0 0 1<br/>

### 思路：
1. 我们首先对每个数字进行长度为 1，2，3 . . . 的左右斜线和进行储存<br/><br/>
2. 我们用 dp[i][j][cnt][0] 表示以 (i , j)为起点长度为cnt 的左斜线和。dp[i][j][cnt][1] 表示以 (i , j)为起点长度为cnt 的右斜线和 <br/><br/>
3. 如果当前位置是  (i , j)，我们想求以 (i , j) 为起点长度为 n 的四角形和。
**sum = dp[i][j][cnt][0] + dp[i][j][cnt][1] + dp[i+cnt][j-cnt][cnt][1] + dp[i+cnt][j+cnt][cnt][0] - mat[i][j] - mat[i+2*cnt][j] - mat[i+cnt][j-cnt] - mat[i+cnt][j+cnt]**。公式看起来有点复杂，代码里的comment图会进行更详细解释。 简单点来说，就是把四角形的四条线加起来<br/><br/>
5. 当然，要注意check 看看是否outbound


### :bulb:代码：
```
class Solution {
    public int[] getBiggestThree(int[][] mat) {
        int n=mat.length,m=mat[0].length;
        int dp[][][][]=new int[n][m][Math.max(n,m)+1][2];
        TreeSet<Integer>tree=new TreeSet<>();
        
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                int r=i,c=j;
                int sum=0;
                int cnt=0;
                while(r<n&&c>=0){
                    sum+=mat[r++][c--];
                    dp[i][j][cnt++][0]=sum;
                }
                
                sum=0;
                r=i;c=j;
                cnt=0;
                while(r<n&&c<m){
                    sum+=mat[r++][c++];
                    dp[i][j][cnt++][1]=sum;
                }
                
            }
        }

        
        
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                tree.add(mat[i][j]);//单个需要进行特殊处理
                for(int cnt=1;cnt<=n;cnt++){
                    if(i+cnt*2>=n||j-cnt<0||j+cnt>=m){//检查outbound
                        break;
                    }
                    int sum=dp[i][j][cnt][0] + dp[i][j][cnt][1] + dp[i+cnt][j-cnt][cnt][1] + dp[i+cnt][j+cnt][cnt][0] - mat[i][j] - mat[i+2*cnt][j] - mat[i+cnt][j-cnt] - mat[i+cnt][j+cnt];
                    tree.add(sum);
                }
            }
        }
        
          
        //转换回array
        List<Integer>list=new ArrayList<>(tree);
        Collections.reverse(list);
        int res[]=new int[Math.min(3,list.size())];
        for(int i=0;i<res.length;i++){
            res[i]=list.get(i);
        }
        return res;
    }
}

//假设以下数组

//0 2 0 
//3 0 5     如果我们求长度是1的四角新，这里的例子由 （2 3 4 5）组成，它的和是 14 
//0 4 0     dp[0][1][1][0] = 2+3 = 5   dp[0][1][1][1] = 2+5 = 7
//          dp[1][0][1][1] = 3+4 = 7   dp[1][2][1][0] = 5+4 = 9
//          由于四个角被重复计算，我们需要最后把他们减去
//          (2+3) + (3+4) + (2+5) + (5 + 4) - (2 + 3 + 4 + 5) = 28 - 14 = 14

//简单来说，就是把四角形的四条线加起来
```
#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(nm min(n,m))
  - 空间复杂度：O(nm min(n,m))
<br/><br/>



## 1879 Minimum XOR Sum of Two Arrays
#### 题意：
>给你两个数组A 和 B，你可以对B的顺序进行任何的调整。
>现在你要最小化 (A[0] xor B[0]) + (A[1] xor B[1]) + . . . . . . +(A[n-1] xor B[n-1])


### 思路：

 1. 当看到 n < 14 的时候，第一个想法就是NP 暴力穷搜或者是使用Bit 进行压缩的DP<br/><br/>
 2. 这题我们可以使用bit 进行状态压缩 （这里使用 DFS + Memo），我们的state表达的是当前数组 B 还有哪些数是可取的。如果当前的state是 1011，这就表示 B[0]，B[1]，B[3] 是可取的<br/><br/>
 3.  我们遍历数组A的每个index，从0开始，A[0] 可以跟数组B的任何一个index进行匹配。假设 n = 4 以及state一开始是1111，那么B 可取的是 B[0]，B[1]，B[2]，B[3]。如果A[0] 和 B[0] 进行匹配，那么state就变成1110。如果A[0] 和B[1] 进行匹配，那么我们的state就变成了1101。然后我们把index 加1和把新的state递归下去。对于A[1]，他能跟剩余的B里的三个数字其中一个进行匹配，以此类推，当数字都匹配完时 return 0<br/><br/>
 4. 如何 **turn off** 其中一个 bit ？ **state = state ^ (1 << i)** <br/><br/>
 5. 如何check ith bit 是否是 1 ？ **if ((state & (1 << i)) != 0)**<br/><br/>
### :bulb:代码：
```
class Solution {
  int dp[][];
  public int minimumXORSum(int[] A, int[] B) {
    dp = new int[A.length + 1][(1 << B.length) + 10];
   
    for (int i = 0; i < dp.length; i++) {
      Arrays.fill(dp[i], -1);// -1 表示此状态还没打卡
    }
    
    int state = (1 << A.length) - 1;
    // 如果n=3, 1<<n = 8, (1<<n)-1 的 二进制则是 111
    
    return dfs(A, B, 0, state);
  }

  public int dfs(int A[], int B[], int index, int state) {
    if (index >= A.length) {//终止条件
      return 0;
    }
    
    if (dp[index][state] != -1) return dp[index][state];//此状态已打卡
    
    int res = Integer.MAX_VALUE;
    for (int i = 0; i < B.length; i++) {
      if ((state & (1 << i)) != 0) {//检查当前第 ith bit是否是1,1 表示可以取当前B这个数字
       //匹配 A[index] 和 B[i], 得到分数 A[index] ^ B[i]
       // newstate = state^(1<<i)
        res = Math.min(res, (A[index] ^ B[i]) + dfs(A, B, index + 1, state ^ (1 << i)));
      }
    }
    dp[index][state] = res;
    return res;
  }
}
```
#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(n 2^n)
  - 空间复杂度：O(n 2^n)
<br/><br/>

