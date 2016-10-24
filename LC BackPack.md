#### LintCode Backpack

**Description**   
>Given n items with size Ai, an integer m denotes the size of a backpack. How full you can fill this backpack?
>Example
If we have 4 items with size [2, 3, 5, 7], the backpack size is 11, we can select [2, 3, 5], so that the max size we can fill this backpack is 10. If the backpack size is 12. we can select [2, 3, 7] so that we can fulfill the backpack.
>
>You function should return the max size we can fill in the given backpack.

**[题目链接](http://www.lintcode.com/en/problem/backpack/)**  
**Solution**  
**思路**  
由递归递归动归对的state function。这道题用递归怎么做呢？物品不能重复出现，f与nums的size有关，去掉一个物品后，size会减少。显然f与target的大小有关。    
所以，f[i][j] = f[i - 1][j], do not add i bag  
    f[i][j] = f[i - 1][j - nums[i - 1]], add i to bag  
    取两者最大值  
f[i][j]是前i个物品装size为j的背包能取得的最大值。

**代码**   
```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        // write your code here
        if (A == null || A.length == 0) {
            return 0;
        }
        // let j is the last index of the sequence
        // let m is the size of the pack
        // let f(i, m) is the largest value
        // f(i, m) = f(i - 1, m) if we do not add i to bag
        // f(i, m) = f(i - 1, m - A[i]) + A[i] if we add i to bag
        // select the max value from these two
        int[][] dp = new int[A.length + 1][m + 1];
        for (int i= 0; i <= A.length; i++) {
            dp[i][0] = 0;
        }
        for (int i = 0; i <= m; i++) {
            dp[0][i] = 0;
        }
        for (int i = 1; i <= A.length; i++) {
            for (int j = 1; j <= m; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j >= A[i - 1]) {
                    dp[i][j] = Math.max(dp[i][j], dp[i- 1][j - A[i - 1]] + A[i - 1]);
                }
                //System.out.println(i + " " + j + " " + dp[i][j]);
            }
        }
        return dp[A.length][m];
    }
}
```
* * *
####  Backpack II

**Description**   
>Given n items with size Ai and value Vi, and a backpack with size m. What's the maximum value can you put into the backpack?
>Given 4 items with size [2, 3, 5, 7] and value [1, 5, 2, 4], and a backpack with size 10. The maximum value is 9.

**[题目链接](http://www.lintcode.com/en/problem/backpack-ii/)**  
**Solution**  
**思路**  
同样由递归到动归推导状态方程。f[i][j]为前i个值size为j时的最大值。  
f[i][j] = f[i - 1][j], if do not add i to backpack  
f[i][j] = f[i][j -  A[i - 1]] + V[i - 1], if add i to backpack    
取两者的最大值  


**代码**   
```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A & V: Given n items with size A[i] and value V[i]
     * @return: The maximum value
     */
    public int backPackII(int m, int[] A, int V[]) {
        // write your code here
        if (m == 0 || A == null || A.length == 0 || V == null || V.length == 0) {
            return 0;
        }
        // f[i][j] is the largest value with i items in bag with size of j
        // f[i][j] = f[i - 1][j]
        // f[i][j] = f[i - 1][j - A[i - 1]] + V[i - 1]
        int[][] dp = new int[A.length + 1][m + 1];
        for (int i = 0; i <= A.length; i++) {
            dp[i][m] = 0;
        }
        for (int j = 0; j <= m; j++) {
            dp[0][j] = 0;
        }
        for (int i = 1; i <= A.length; i++) {
            for (int j = 1; j <= m; j++) {
                dp[i][j] = dp[i - 1][j]; // if do not add i
                if (j >= A[i - 1]) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - A[i - 1]] + V[i - 1]);
                    //System.out.println(i +" " + j + " " + dp[i][j]);
                }
            }
        }
        return dp[A.length][m];
    }
}
```
* * *
#### LintCode BackPack III

**Description**   
>Given n kind of items with size Ai and value Vi( each item has an infinite number available) and a backpack with size m. What's the maximum value can you put into the backpack?
Notice
You cannot divide item into small pieces and the total size of items you choose should smaller or equal to m.
Example
>
>Given 4 items with size [2, 3, 5, 7] and value [1, 5, 2, 4], and a backpack with size 10. The maximum value is 15.

**[题目链接]()**  
**Solution**  
**思路**  
因为物品可以无限取，因此结果与物品的size无关，至于背包的大小有关。  
f[i], size为i的背包能装的最大值  
f[i] = f[i - 1], do not add i to bag  
f[i] = max(f[i - A[j]] + V[j]) | j in [0, nums.length - 1]  
取最大值  

**代码**   
```java
public class SolutionBackPackIII {
    public int backPack(int A[], int[] V, int target) {
        if (A == null || A.length == 0 || V == null || V.length == 0 || A.length != V.length || target == 0) {
            return 0;
        }
        int[] dp = new int[target + 1];
        for (int i = 1; i <= target; i++) {
            dp[i] = dp[i - 1]; // if don't add i to backPack
            for (int j = 0; j < A.length; j++) {
                if (i >= A[j]) {
                    dp[i] = Math.max(dp[i], dp[i - A[j]] + V[j]);// if we add i to backPack
                }
            }
        }
        return dp[target];
    }
    public static void main(String[] args) {
        SolutionBackPackIII s = new SolutionBackPackIII();
        int[] A = new int[]{2, 3, 5, 7};
        int[] V = new int[]{1, 5, 2, 4};
        System.out.println(s.backPack(A, V, 10) + ", should be " + 15);
    }
}
```
* * *

#### LintCode Backpack VI

**Description**   
>Given an integer array nums with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.
>
>Notice
>
>The different sequences are counted as different combinations.

>Have you met this question in a real interview? Yes
>Example
>Given nums = [1, 2, 4], target = 4
>
>The possible combination ways are:
```
[1, 1, 1, 1]
[1, 1, 2]
[1, 2, 1]
[2, 1, 1]
[2, 2]
[4]
```
>return `6`

**[题目链接](http://www.lintcode.com/en/problem/backpack-vi/)**  
**Solution**  
**思路**  
由递归推导动归对的state function. 在位置i，缩小size的情况下，会产生什么样的递推关系。   
递归，是由大到小，最小到边界情况返回值，而大size的解取决于小size的解。  
设target为i，数组为nums时的组合数。 f[i] = sum(f[i - nums[k]]) | 0 =< k < nums.length。  

**代码**   
```java
public class Solution {
    /**
     * @param nums an integer array and all positive numbers, no duplicates
     * @param target an integer
     * @return an integer
     */
    public int backPackVI(int[] nums, int target) {
        
        if (nums == null || nums.length == 0 || target == 0) {
            return 0;
        }
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (int j = 0; j < nums.length; j++) {
                if (i >= nums[j]) {
                    dp[i] += dp[i - nums[j]];
                }
            }
        }
        
        return dp[target];
    }
}
```
* * *
