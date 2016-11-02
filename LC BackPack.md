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
由递归推导动归的state function。
dfs(int[] A, int target, int pos), f与target大小和pos(size)有关。   
所以，f[i][j] = f[i - 1][j], do not add i bag  
    f[i][j] = f[i - 1][j - nums[i - 1]], add i to bag  
    取两者最大值  
f[i][j]是前i个物品装size为j的背包能取得的最大值。

**代码**   
**首先看memorization-dfs**  
```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPack(int m, int[] A) {
        if (A == null || A.length == 0 || m == 0) {
            return 0;
        }
        int[][] memo = new int[A.length][m + 1];
        for (int[] a : memo) {
            Arrays.fill(a, -1);
        }
        return dfs(A, m, A.length - 1, memo);
    }
    public int dfs(int[] A, int m, int i, int[][] memo) {
        if (i < 0) {
            return 0;
        }
        if (memo[i][m] > -1) {
            return memo[i][m];
        }
        memo[i][m] = dfs(A, m, i - 1, memo);
        if (m >= A[i]) {
            memo[i][m] = Math.max(memo[i][m], dfs(A, m - A[i], i - 1, memo) + A[i]);
        }
        return memo[i][m];
    }
}
```
  
**DP** 
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
**优化**   
dp[i][j]只跟上一层i - 1有关，i - 1层之前都不会再用到，因此只需要位于m + 1大小的以为数组即可。但遍历的时候内层m应从大到小，这样访问到的dp[j]等价于dp[i - 1][j];  
**decrease space to O(m)**  
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
        int[] dp = new int[m + 1];
        dp[0] = 0;
        for (int i = 0; i < A.length; i++) {
            for (int j = m; j >= A[i]; j--) {
                if (j >= A[i]) {
                    dp[j] = Math.max(dp[j], dp[j - A[i]] + A[i]);
                }
            }
        }
        return dp[m];
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
同样由递归推导动归的状态方程。f[i][j]为前i个值size为j时的最大值。dfs（int[] A, int[] V, int target, int pos）.  
f[i][j] = f[i - 1][j], if do not add i to backpack  
f[i][j] = f[i][j -  A[i - 1]] + V[i - 1], if add i to backpack    
取两者的最大值  


**代码**   
**DFS memorization**  
```java
public class Solution {
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    public int backPackII(int m, int[] A, int[] V) {
        if (A == null || A.length == 0 || m == 0) {
            return 0;
        }
        int[][] memo = new int[A.length][m + 1];
        for (int[] a : memo) {
            Arrays.fill(a, -1);
        }
        return dfs(A, V, m, A.length - 1, memo);
    }
    public int dfs(int[] A, int[] V, int m, int i, int[][] memo) {
        if (i < 0) {
            return 0;
        }
        if (memo[i][m] > -1) {
            return memo[i][m];
        }
        memo[i][m] = dfs(A, V, m, i - 1, memo);
        if (m >= A[i]) {
            memo[i][m] = Math.max(memo[i][m], dfs(A, V, m - A[i], i - 1, memo) + V[i]);
        }
        return memo[i][m];
    }
}
```
**DP, O(m * n) space**  
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
**优化，滚动数组， O(m) space**  
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
        int[][] dp = new int[2][m + 1];
        for (int i = 1; i <= A.length; i++) {
            for (int j = 1; j <= m; j++) {
                dp[i % 2][j] = dp[(i - 1) % 2][j];
                if (j >= A[i - 1]) {
                    dp[i % 2][j] = Math.max(dp[i % 2][j], dp[(i - 1) % 2][j - A[i - 1]] + V[i - 1]);
                }
            }
        }
        return dp[A.length % 2][m];
    }
}
```
**进一步优化，O(m) space**     
从递推公式可以看出：
f[i][j] = f[i - 1][j], if do not add i to backpack  
f[i][j] = f[i][j -  A[i - 1]] + V[i - 1], if add i to backpack      
f[i][j]仅跟i层和i - 1层有关，因此m + 1大小的数组即可，但内层需要反向遍历（这样f[j] <=> f[i - 1][j]），否则会重复选取，参看BackPackIII。
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
        int[] dp = new int[m + 1];
        for (int i = 1; i <= A.length; i++) {
            for (int j = m; j >= A[i - 1]; j--) {
                dp[j] = Math.max(dp[j], dp[j - A[i - 1]] + V[i - 1]);
            }
        }
        return dp[m];
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
因为物品可以无限取，因此结果与物品的size无关，只与背包的大小有关。
```
dfs(int[] A, int target) {
	// goal state
	for (int i = 0; i < A.length; i++) {
		dfs(A, target)
		if (target > A[i]) {
			dfs(A, target - A[i]);
		}
	}
}
```
f[i], size为i的背包能装的最大值  
f[i] = f[i - 1], do not add i to bag  
f[i] = max(f[i - A[j]] + V[j]) | j in [0, nums.length - 1]  
取最大值  

**代码**   
```java
import java.util.*;
public class SolutionBackPackIII {
    public int backPack(int A[], int[] V, int target) {
        if (A == null || A.length == 0 || A.length != V.length || target == 0) {
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
	public int backPackDfs(int[] A, int[] V, int target) {
		if (A == null || A.length == 0 || A.length != V.length || target == 0) {
            return 0;
        }
		int[][] memo = new int[A.length][target + 1];
		for (int[] a : memo) {
			Arrays.fill(a, -1);
		}
        return dfs(A, V, target, A.length - 1, memo);
	}
	public int dfs(int[] A, int[] V, int m, int i, int[][] memo) {
		if (i < 0) {
			return 0;
		}
		if (m == 0) {
			return 0;
		}
		if (memo[i][m] > -1) {
			return memo[i][m];
		}
		memo[i][m] = dfs(A, V, m, i - 1, memo);
		if (m >= A[i]) memo[i][m] = Math.max(memo[i][m], dfs(A, V, m - A[i], i, memo) + V[i]);
		return memo[i][m];
	}
	public int backPackDp(int[] A, int[] V, int target) {
		
		int[] f = new int[target + 1];
		for (int i = 0; i < A.length; i++) {
			for (int j = A[i]; j <= target; j++) {
				f[j] = Math.max(f[j], f[j - A[i]] + V[i]);
			}
		}
		return f[target];
	}
    public static void main(String[] args) {
        SolutionBackPackIII s = new SolutionBackPackIII();
        int[] A = new int[]{2, 3, 5, 7};
        int[] V = new int[]{1, 5, 2, 4};
        System.out.println(s.backPack(A, V, 10) + ", should be " + 15);
		System.out.println(s.backPackDfs(A, V, 10) + ", should be " + 15);
		System.out.println(s.backPackDp(A, V, 10) + ", should be " + 15);
    }
}
```
* * *
#### Backpack IV

**Description**   
>Given n items with size nums[i] which an integer array and all positive numbers, no duplicates. An integer target denotes the size of a backpack.   Find the number of possible fill the backpack.  
Each item may be chosen unlimited number of times.  
>Example  
>Given candidate items [2,3,6,7] and target 7,   
>A solution set is:  [7]   
>[2, 2, 3]   
>return 2   

**[题目链接]()**  
**Solution**  
**思路**  
递归推导动归状态转移方程。
f[i][j], pos = i and target = j  
f[i][j] = f[i - 1][j], if we don't choose i
f[i][j] += f[i][j - A[i]], if we choose i and j >= A[i]  
因为f[i][j]仅与上一层i - 1和这一层i有关系，优化后，我们可以将二维数组降维到一维。因为允许重复取，内层正向遍历。  
**代码**   
```java
public class SolutionBackPackIV {
	// first method,
	public int backPack(int A[], int target) {
        if (A == null || A.length == 0 || target == 0) {
            return 0;
        }
        int[][] dp = new int[A.length + 1][target + 1];
		for (int i = 0; i <= A.length; i++) {
			dp[i][0] = 1;
		}
        
		for (int i = 1; i <= A.length; i++) {
			for (int j = 1; j <= target; j++) {
				dp[i][j] = dp[i - 1][j];
				if (j >= A[i - 1]) {
					dp[i][j] += dp[i][j - A[i - 1]];
				}
			}
		}
        return dp[A.length][target];
    }
    public int backPack2(int A[], int target) {
        if (A == null || A.length == 0 || target == 0) {
            return 0;
        }
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int i = 0; i < A.length; i++) {
			for (int j = A[i]; j <= target; j++) {
				dp[j] += dp[j - A[i]]; // refer to row i - 1 and updated row i
			}
		}
        return dp[target];
    }
    // second method, memorization dfs
    public int dfs(int[] A, int target, int pos) {
        if (pos < 0) {
			if (target == 0) {
				return 1;
			}
			return 0;
		}
		if (target == 0) {
			return 1;
		}
		int res = 0;
		res += dfs(A, target, pos - 1); // refer to row i - 1
		if (target >= A[pos]) {
			res += dfs(A, target - A[pos], pos); // refer to row i
		}
		return res;
    }
    public static void main(String[] args) {
        SolutionBackPackIV s = new SolutionBackPackIV();
        int[] A = new int[]{1,2,4};
        System.out.println(s.backPack(A, 4) + ", should be " + 4);
        System.out.println(s.backPack2(A, 4) + ", should be " + 4);
        System.out.println(s.dfs(A, 4, 2) + ", should be " + 4);
    }
}

```
* * *


#### Backpack V 

**Description**   
>Given n items with size nums[i] which an integer array and all positive numbers. An integer target denotes the size of a backpack. Find the number of possible fill the backpack.  
>Each item may only be used once.  
>Example  
>Given candidate items [1,2,3,3,7] and target 7,  
>A solution set is:  [7]  
> [1, 3, 3]    
>  return 2  

**[题目链接]()**  
**Solution**  
**思路**  
递归推导动归状态转移方程。
f[i][j], pos = i and target = j  
f[i][j] = f[i - 1][j], if we don't choose i
f[i][j] += f[i - 1][j - A[i]], if we choose i and j >= A[i]  
因为f[i][j]仅与上一层i - 1有关系，优化后，我们可以将二维数组降维到一维。因为不允许重复取，内层反向遍历。
**代码**   
```java
public class SolutionBackPackV {
	// first method,
	public int backPack(int A[], int target) {
        if (A == null || A.length == 0 || target == 0) {
            return 0;
        }
        int[][] dp = new int[A.length + 1][target + 1];
		for (int i = 0; i <= A.length; i++) {
			dp[i][0] = 1;
		}
        
		for (int i = 1; i <= A.length; i++) {
			for (int j = 1; j <= target; j++) {
				dp[i][j] = dp[i - 1][j];
				if (j >= A[i - 1]) {
					dp[i][j] += dp[i - 1][j - A[i - 1]];
				}
			}
		}
        return dp[A.length][target];
    }
    public int backPack2(int A[], int target) {
        if (A == null || A.length == 0 || target == 0) {
            return 0;
        }
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int i = 0; i < A.length; i++) {
			for (int j = target; j >= A[i]; j--) {
				dp[j] += dp[j - A[i]]; // refer to row i - 1 and row i - 1 or, we say, un-updated row i
			}
		}
        return dp[target];
    }
    // second method, memorization dfs
    public int dfs(int[] A, int target, int pos) {
        if (pos < 0) {
			if (target == 0) {
				return 1;
			}
			return 0;
		}
		if (target == 0) {
			return 1;
		}
		int res = 0;
		res += dfs(A, target, pos - 1); // refer to row i - 1
		if (target >= A[pos]) {
			res += dfs(A, target - A[pos], pos - 1); // refer to row i - 1 as well
		}
		return res;
    }
    public static void main(String[] args) {
        SolutionBackPackV s = new SolutionBackPackV();
        int[] A = new int[]{1,2,3,3,7};
        System.out.println(s.backPack(A, 7) + ", should be " + 2);
        System.out.println(s.backPack2(A, 7) + ", should be " + 2);
        System.out.println(s.dfs(A, 7, 4) + ", should be " + 2);
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
**Memorization-dfs**  
```java
public class Solution {
    /**
     * @param nums an integer array and all positive numbers, no duplicates
     * @param target an integer
     * @return an integer
     */
    public int backPackVI(int[] A, int target) {
        // Write your code here
        // corner case
        if (A == null || A.length == 0 || target == 0) {
            return 0;
        }
        int[] memo = new int[target + 1];
        Arrays.fill(memo, -1);
        return dfs(A, target, memo);
    }
    public int dfs(int[] A, int target, int[] memo) {
        // goal state
        if (target == 0) {
            return 1;
        }
        if (memo[target] > -1) {
            return memo[target];
        }
        int count = 0;
        for (int i = 0; i < A.length; i++) {
            if (target >= A[i]) {
                count += dfs(A, target - A[i], memo);
            }
        }
        memo[target] = count;
        return count;
    }
}
```
* * *
