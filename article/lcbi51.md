
# LeetCode BiWeekly Contest 51
这是对早上刚刚结束 LeetCode 第51场周赛的简单讲解和评论。以下会包括对**Leetcode双周赛 51**的每一题的题解，代码，难度分析，题目类型，以及一些个人对这题的看法等，希望读者们喜欢！<br/><br/>

**:bulb:注意** ： 题解并不一定都是最优解（代码以pass 了 LeetCode 的 Oline Judge 为准），文章初衷是给读者们提供一定的想法和问题的解决方案，以及以保持速更为主要目的

 1844. [Replace All Digits with Characters](https://leetcode.com/problems/replace-all-digits-with-characters)
 1845. [Seat Reservation Manager](https://leetcode.com/problems/seat-reservation-manager)
 1846. [Maximum Element After Decreasing and Rearranging](https://leetcode.com/problems/maximum-element-after-decreasing-and-rearranging)
 1847. [Closest Room](https://leetcode.com/problems/closest-room)



## 前言 : 参加Contest 的好处

 1. 每周抽点时间去练习和学习新的题目这对于个人的编程能力与问题解决能力都有很大的帮助
 2. 在规定的时间内解决一定的题目，这和真实的**OA (Online Assessment)** 或者 **Onsite** 面试是一样的，可以把它当作一个很好的 Mock 练习机会
 3. 当你有碰到做不出的题目的时候，你能知道自己的弱项在哪里，然后可以根据这进行加强和练习，比赛是一面给自己的镜子
<br/><br/><br/>

## 对本场比赛的一些看法
本场比赛前三题难度简单，属于手速场。第四题有点难度但题目质量感觉非常不错，可以深入学习一下<br/><br/>

 **1844**. 一道简单的暴力题，会编程的人基本都要会的入门题目<br/>
 **1845**. 对于**Java** 使用者来说 （小编忠爱渣娃），这是一个TreeMap/TreeSet 的应用题。当然这题也能使用PriorityQueue 来做，本题小编会提供**两个解**<br/>
 **1846**. 一道简单的贪心题目，排一下序就好 （有点惊讶第三题会如此简单，还以为会不会有什么天坑给小编跳）<br/>
 **1847**. 比较好的一题，可以深入学习一下。小编比赛时用的方法比较复杂 (Segment Tree + Binary Search 读者们有兴趣可以试试)，代码比较丑陋的关系赛后重新写了一份TreeSet + 双指针的解法。这里可以对TreeSet/TreeMap 的floor 和 ceiling 这个API 了解一下，非常好用<br/><br/>


 

## 1844 Replace All Digits with Characters

#### 题意：
>给你一个字串，它的偶数index 是字母，奇数index 是数字。如果是字母，我们直接加进去答案里。如果是数字，我们取出前面的字母再把它shift 当前数字值的位置，把最终的这个字母加进去答案里<br/>
>a1c1e1 ： abcdef <br/><br/>
>s[1] -> shift('a',1) = 'b'<br/>
>s[3] -> shift('c',1) = 'd'<br/>
> s[5] -> shift('e',1) = 'f'<br/>


### 思路：
1. 简单一个loop 过去就行了，如果是**Even Index**我们直接把字母加进result 里面，如果是**Odd Index**，我们把前一个字母取出来再转换成我们需要的字母即可<br/>
2. **Shift** 的时候我们可以把前面的字母先转换成 0-25 的数字，然后用这个数字加上当前的数字再取模26，最后转换回字母

### :bulb:代码：
```
class Solution {
    public String replaceDigits(String s) {
        StringBuilder str = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);//当前字母
            if (i % 2 == 0) {//直接加进去
                str.append(c + "");
            } else {
                int pre = s.charAt(i - 1) - 'a';
                pre = (pre + (c - '0')) % 26;//变成数字后再转换
                str.append((char)(pre + 'a'));
            }
        }
        return str.toString();
    }
}
```

#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(n)
  - 空间复杂度：O(1)
<br/><br/>



## 1845 Seat Reservation Manager
#### 题意：
>一开始你有 1 到 n 的数字。现在有两个function，reserve() 会取出最小你先有的最小的那个数字，unreserve(id) 会把id 这个数加进你有的数字里面<br/>



### :bulb:代码：

### 思路：

 1. 这题是对 TreeMap/TreeSet 的基本应用题，我们只需要用TreeSet 里的firstKey() 这个API 就行<br/><br/>

2. 这题也可以用PriorityQueue，每次取出最小再把得到的数字加进去里面。但是考虑到有重复数字的可能性，我们在remove的时候注意要一次把所有重复的给移除掉，因为它有可能重复加同一个数字。TreeMap/TreeSet 的好处就是可以直接处重<br/><br/>


### :bulb:代码1：
```
class SeatManager {
    TreeSet < Integer > tree = new TreeSet < > ();
    public SeatManager(int n) {
        //一开始拥有 [1 : n]的数字
        for (int i = 1; i <= n; i++) {
            tree.add(i);
        }
    }

    public int reserve() {
        Integer first = tree.first(); //TreeSet 的first 就是最小的那一个
        tree.remove(first);
        return first;
    }

    public void unreserve(int id) {
        tree.add(id); //加进去
    }
}

/**
 * Your SeatManager object will be instantiated and called as such:
 * SeatManager obj = new SeatManager(n);
 * int param_1 = obj.reserve();
 * obj.unreserve(seatNumber);
 */
```
#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(nlogn)
  - 空间复杂度：O(n) 
<br/><br/>

### :bulb:代码2：
```
class SeatManager {
    PriorityQueue < Integer > pq = new PriorityQueue < > ();
    public SeatManager(int n) {
        for (int i = 1; i <= n; i++) {
            pq.add(i);
        }
    }

    public int reserve() {
        int mn = pq.poll(); //最小
        while (pq.size() > 0 && pq.peek() == mn) { //可能有重复的数字，我们要直接移除
            pq.poll();
        }
        return mn;
    }

    public void unreserve(int id) {
        pq.add(id);
    }
}

/**
 * Your SeatManager object will be instantiated and called as such:
 * SeatManager obj = new SeatManager(n);
 * int param_1 = obj.reserve();
 * obj.unreserve(seatNumber);
 */
```
#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(nlogn)
  - 空间复杂度：O(n + number of unreserve() call) 
<br/><br/>


## 1846 Maximum Element After Decreasing and Rearranging
#### 题意：
>给你一个数组，我们有两种操作。1. 随便rearrange 我们的数组 2. 减少其中的一个数字<br/>
>并且第一个数得是1以及最终的数组每两个相邻数字的abs 差不能大于1，问这个数组最终最大的数是多少<br/>
>[2,2,1,2,1] 变成 [1,2,2,2,1]，其中最大的是2 <br/>
>[100,1,1000] 变成 [1,2,3]，其中最大的是3 <br/>




### 思路：

 1. 既然可以随便rearrange，那我们第一步肯定是排序啦 <br/><br/>
 2. 第一个数字既然是1 的加上我们只有减少的操作的关系，第一个数字几乎已经决定了整个Array的大势走向<br/><br/>
 3. 我们首先使第一个数字是1，然后贪心的改变数字，如果当前数字**A[i]** 比前面的大于1，我们使他变成 **A[i-1]+1**，如果已经符合条件，即 **A[i] -A[i-1]<=1**，我们就不用管他，因为没理由再去对他进行减少。加上我们的Array 是排序好的，就算不改变他也不会影响到后面<br/><br/>

### :bulb:代码：
```
class Solution {
    public int maximumElementAfterDecrementingAndRearranging(int[] A) {
        Arrays.sort(A);
        int p = 0;//前面的数字，设他为0 我们的loop 就直接可以从0 开始
        
        for (int i = 0; i < A.length; i++) {
            if (A[i] - p > 1) {
                A[i] = p + 1;
            }
            p = A[i];
        }


        return A[A.length - 1];
    }
}

/**
 * Your SeatManager object will be instantiated and called as such:
 * SeatManager obj = new SeatManager(n);
 * int param_1 = obj.reserve();
 * obj.unreserve(seatNumber);
 */
```
#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(nlogn)
  - 空间复杂度：O(1) 
<br/><br/>

## 1847 Closest Room

#### 题意：
>给你两个数组，每个数组都是2D 数组。rooms = [[2,2],[1,2],[3,2]], queries = [[3,1],[3,3],[5,2]].<br/>
>rooms[i] = [roomIdi, sizei]  queries[i] = [preferredj, minSizej]<br/><br/>
>对于每一个query，我们要找到一个room，他的size>= minSize，并且id 是与它最接近的。如果有两个一样距离的id，我们res[i] 使用最小的那一个。如果没有比他大的房子，res[i]=-1

### 思路：
1. 这题小编在比赛中用的是Max Segment Tree + TreeMap + BinarySearch, 虽然AC 了但是代码并不太好看 （读者们有兴趣可以尝试一下），赛后看了一些其他人的解，换了以下的解法，使用TreeSet + 双指针<br/><br/>
2. 我们可以先把query 和 rooms 都根据他们的size sort 一下<br/><br/>
3. 我们可以一边走我们的query，然后用另一个指针指向rooms，假设rooms的指针现在在 index **j**，这就表示 **A[j : n-1]** 这里面的room 的size 都大于或者等于当前query 的size。换而言之，我们只需要在 **A[j : n-1]** 找到一个room id 与当前query 最近的一个即可。我们可以用一个TreeSet 一边走一边remove<br/><br/>
4. 这就是我们说所的**off line 离线处理**，小编原来的方法是 **online 处理**

### :bulb:代码：
```
lass Solution {
    public int[] closestRoom(int[][] A, int[][] queries) {
        int res[] = new int[queries.length];
        Arrays.fill(res, -1);
        
        int q[][] = new int[queries.length][3];//做一个新的query，这样我们sort的时候可以保持它原来的顺序
        
        TreeSet < Integer > tree = new TreeSet < > ();
        for (int i = 0; i < q.length; i++) {
            q[i][0] = queries[i][0];
            q[i][1] = queries[i][1];
            q[i][2] = i;
        }


        Arrays.sort(A, (a, b) - > {
            return a[1] - b[1];
        }); //根据size sort

        Arrays.sort(q, (a, b) - > {
            return a[1] - b[1];
        }); //根据size sort

        for (int i = 0; i < A.length; i++) {//把所有的房间都加进TreeSet 里面
            tree.add(A[i][0]);
        }


        int j = 0;//rooms 的指针

        for (int i = 0; i < q.length; i++) {
            int id = q[i][0], size = q[i][1], index = q[i][2];
            
            while (j < A.length && A[j][1] < size) {//移动指针和update TreeSet
                tree.remove(A[j][0]);
                j++;
            }

	    //A[j : n-1] 里的size 都比当前query的大，找到一个id 离当前query 最近的
	    //用到 TreeSet/TreeMap 里的floor 和 ceil，如有不懂可Goold 一下其API
			 
            Integer floor = tree.floor(id);
            Integer ceil = tree.ceiling(id);

            if (floor != null && ceil != null) {
                int dif1 = id - floor;
                int dif2 = ceil - id;
                if (dif1 <= dif2) {
                    res[index] = floor;
                } else {
                    res[index] = ceil;
                }
            } else if (floor != null) {
                res[index] = floor;
            } else if (ceil != null) {
                res[index] = ceil;
            }

        }

        return res;
    }
}
```
#### 空间复杂度和时间复杂度：
  - 时间复杂度：O(nlogn)
  - 空间复杂度：O(n) 
<br/><br/>
