#### 374. Guess Number Higher or Lower

__Description__   
>We are playing the Guess Game. The game is as follows:
>
>I pick a number from 1 to n. You have to guess which number I picked.
>
>Every time you guess wrong, I'll tell you whether the number is higher or lower.
>
>You call a pre-defined API guess(int num) which returns 3 possible results (-1, 1, or 0):
```
-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!
 ```
>Example:
```
n = 10, I pick 6.
```

>Return 6.

__Solution__  
**思路** 
简单的binary search   
```java
/* The guess API is defined in the parent class GuessGame.
   @param num, your guess
   @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
      int guess(int num); */

public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int start = 1;
        int end = n;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (guess(mid) < 0) {
                end = mid;
            } else if (guess(mid) > 0) {
                start = mid;
            } else {
                return mid;
            }
        }
        if (guess(start) == 0) {
            return start;
        }
        if (guess(end) == 0) {
            return end;
        }
        return -1;
    }
}
```
* * * 

#### 375. Guess Number Higher or Lower II  

**Description**   
>We are playing the Guess Game. The game is as follows:
>
>I pick a number from 1 to n. You have to guess which number I picked.
>
>Every time you guess wrong, I'll tell you whether the number I picked is higher or lower.
>
>However, when you guess a particular number x, and you guess wrong, you pay $x. You win the game when you guess the number I picked.
>
>Example:
>
>n = 10, I pick 8.
>
>First round:  You guess 5, I tell you that it's higher. You pay $5.
>Second round: You guess 7, I tell you that it's higher. You pay $7.
>Third round:  You guess 9, I tell you that it's lower. You pay $9.
>
>Game over. 8 is the number I picked.
>
>You end up paying $5 + $7 + $9 = $21.
>Given a particular n ≥ 1, find out how much money you need to have to guarantee a win.

**[题目链接](https://leetcode.com/problems/guess-number-higher-or-lower-ii/)**  
**Solution**  
**思路**  
分析：题目要求在最坏情况下我们至少要付出多少钱才能赢。  枚举所有策略。  
所谓最坏情况是在每个策略中每一步均猜错，但在得知higher或lower之后选择最优策略去猜。  
以`1， 2， 3`为例，3种策略分别为（1 + 2 = 3， 2， 3 + 1 = 4），最优策略为2。`1,2,3,4`，最优策略是4。
```
				1234 
		  / 1  |2   | 3 \4
		234  1|34 12|4   123
		 |     |    |     |
         3     3    2     2
```
本题用dfs + memorization的方法解决。
**代码**   
```java
public class Solution {
    public int solve(int[][] dp, int l, int r) {
        if (l >= r) {
            return 0;
        }
        if (dp[l][r] != Integer.MAX_VALUE) {
            return dp[l][r];
        }
        for (int i = l; i <= r; i++) {
            dp[l][r] = Math.min(dp[l][r], i + Math.max(solve(dp, i + 1, r), solve(dp, l, i - 1)));
        }
        return dp[l][r];

    }
    public int getMoneyAmount(int n) {
        int[][] dp = new int[n + 1][n + 1];
        for(int[] row: dp){
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        return solve(dp, 1, n);
    }
```
**dp**  
```java
public class Solution {
    public int getMoneyAmount(int n) {
        int[][] dp = new int[n + 2][n + 2];
        
        for (int len = 1; len < n; len++) {
            for (int i = 1; i + len <= n; i++) {
                int min = Integer.MAX_VALUE;
                int j = i + len;
                for (int k = i; k <= j; k++) {
                    min = Math.min(min, k + Math.max(dp[i][k - 1], dp[k + 1][j]));
                }
                dp[i][j] = min;
            }
        }
        return dp[1][n];
    }
}
```
* * *
####  Interval Minimum Number

**Description**   
>Given an integer array (index from 0 to n-1, where n is the size of this array), and an query list. Each query has two integers [start, end]. For each query, calculate the minimum number between index start and end in the given array, return the result list.
>Example
For array [1,2,7,8,5], and queries [(1,2),(0,4),(2,4)], return [2,1,5]

**[题目链接](https://www.lintcode.com/en/problem/interval-minimum-number/#)**  
**Solution**  
**思路**  
Segment Tree. Build and query.  

**代码**   
```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */
class SegmentNode {
     int start;
     int end;
     int min;
     SegmentNode left;
     SegmentNode right;
     public SegmentNode(int start, int end, int min) {
         this.start = start;
         this.end = end;
         this.left = null;
         this.right = null;
         this.min = min;
     }
}
public class Solution {
    /**
     *@param A, queries: Given an integer array and an query list
     *@return: The result list
     */
    
    public ArrayList<Integer> intervalMinNumber(int[] A, 
                                                ArrayList<Interval> queries) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (A == null || A.length == 0 || queries == null || queries.size() == 0) {
            return result;
        }
        SegmentNode root = build(A, 0, A.length - 1);
        for (Interval in : queries) {
            result.add(query(root, in.start, in.end));
        }
        return result;
    }
    public SegmentNode build(int[] A, int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, A[start]);
        }
        SegmentNode root = new SegmentNode(start, end, Integer.MAX_VALUE);
        int mid = (start + end) / 2;
        SegmentNode left = build(A, start, mid);
        SegmentNode right = build(A, mid + 1, end);
        root.left = left;
        root.right = right;
        root.min = Math.min(left.min, right.min);
        //System.out.println(start + " " + end + " " + root.min);
        return root;
    }
    public int query(SegmentNode root, int start, int end) {
        if (start == root.start && end == root.end || root.start == root.end) {
            return root.min;
        }
        
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            int left = query(root.left, start, mid);
            int right = query(root.right, mid + 1, end);
            return Math.min(left, right);
        }
        if (end <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}
```
* * *
#### Interval Sum

**Description**   
>Given an integer array (index from 0 to n-1, where n is the size of this array), and an query list. Each query has two integers [start, end]. For each query, calculate the sum number between index start and end in the given array, return the result list.
>Example
> For array [1,2,7,8,5], and queries [(0,4),(1,2),(2,4)], return [23,9,20]
**[题目链接](https://www.lintcode.com/en/problem/interval-sum/#)**  
**Solution**  
**思路**  
Segment Tree. 

**代码**   
```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */
class SegmentNode {
     int start;
     int end;
     long sum;
     SegmentNode left;
     SegmentNode right;
     public SegmentNode(int start, int end, long sum) {
         this.start = start;
         this.end = end;
         this.left = null;
         this.right = null;
         this.sum = sum;
     }
}
public class Solution {
    /**
     *@param A, queries: Given an integer array and an query list
     *@return: The result list
     */
    public ArrayList<Long> intervalSum(int[] A, 
                                       ArrayList<Interval> queries) {
        // write your code here
        ArrayList<Long> result = new ArrayList<Long>();
        if (A == null || A.length == 0 || queries == null || queries.size() == 0) {
            return result;
        }
        SegmentNode root = build(A, 0, A.length - 1);
        for (Interval in : queries) {
            result.add(query(root, in.start, in.end));
        }
        return result;
    }
    public SegmentNode build(int[] A, int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, A[start]);
        }
        SegmentNode root = new SegmentNode(start, end, 0);
        int mid = (start + end) / 2;
        SegmentNode left = build(A, start, mid);
        SegmentNode right = build(A, mid + 1, end);
        root.left = left;
        root.right = right;
        root.sum = left.sum + right.sum;
        //System.out.println(start + " " + end + " " + root.min);
        return root;
    }
    public long query(SegmentNode root, int start, int end) {
        if (start == root.start && end == root.end || root.start == root.end) {
            return root.sum;
        }
        
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            long left = query(root.left, start, mid);
            long right = query(root.right, mid + 1, end);
            return left + right;
        }
        if (end <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}

```
* * *
####  Interval Sum II

**Description**   
>Given an integer array in the construct method, implement two methods query(start, end) and modify(index, value):
>
>For query(start, end), return the sum from index start to index end in the given array.
>For modify(index, value), modify the number in the given index to value
>Example
>Given array A = [1,2,7,8,5].
>
>query(0, 2), return 10.
>modify(0, 4), change A[0] from 1 to 4.
>query(0, 1), return 6.
>modify(2, 1), change A[2] from 7 to 1.
>query(2, 4), return 14.

**[题目链接](https://www.lintcode.com/en/problem/interval-sum-ii/#)**  
**Solution**  
**思路**  
Segment Tree

**代码**   
```java
class SegmentNode {
    int start;
    int end;
    long sum;
    SegmentNode left;
    SegmentNode right;
    public SegmentNode(int start, int end, long sum) {
        this.start = start;
        this.end = end;
        this.sum = sum;
        this.left = null;
        this.right = null;
    }
}
public class Solution {
    /* you may need to use some attributes here */
    SegmentNode segmentTree;
    
    /**
     * @param A: An integer array
     */
    public Solution(int[] A) {
        // write your code here
        this.segmentTree = build(A, 0, A.length - 1);
    }
    
    /**
     * @param start, end: Indices
     * @return: The sum from start to end
     */
    public long query(int start, int end) {
        // write your code here
        if (segmentTree == null || start > end || start > segmentTree.end || end < segmentTree.start) {
            return Long.MIN_VALUE;
        }
        return query(segmentTree, start, end);
    }
    public long query(SegmentNode root, int start, int end) {
        if (start  == root.start && end == root.end || root.start == root.end) {
            return root.sum;
        }
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            long left = query(root.left, start, mid);
            long right = query(root.right, mid + 1, end);
            return left + right;
        }
        if (start <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
    
    /**
     * @param index, value: modify A[index] to value.
     */
    public void modify(int index, int value) {
        // write your code here
        if (segmentTree == null || index > segmentTree.end || index < segmentTree.start) {
            return;
        }
        modify(segmentTree, index, value);
        
    }
    public void modify(SegmentNode root, int index, int value) {
        if (index > root.end || index < root.start) {
            return;
        }
        if (root.start == root.end) {
            root.sum = value;
            return;
        }
        int mid = (root.start + root.end) / 2;
        if (index <= mid) {
            modify(root.left, index, value);
        } else {
            modify(root.right, index, value);
        }
        root.sum = root.left.sum + root.right.sum;
    }
    public SegmentNode build(int[] A, int start, int end) {
        if (A == null || A.length == 0 || start > end) {
            return null;
        }
        return buildHelper(A, start, end);
    }
    public SegmentNode buildHelper(int[] A, int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, A[start]);
        }
        SegmentNode root = new SegmentNode(start, end, 0);
        int mid = (start + end) / 2;
        SegmentNode left = buildHelper(A, start, mid);
        SegmentNode right = buildHelper(A, mid + 1, end);
        root.left = left;
        root.right = right;
        root.sum = left.sum + right.sum;
        return root;
    }
}
```
* * *
#### Count of Smaller Number

**Description**   
>Give you an integer array (index from 0 to n-1, where n is the size of this array, value from 0 to 10000) and an query list. For each query, give you an integer, return the number of element in the array that are smaller than the given integer.
>Example
>For array [1,2,7,8,5], and queries [1,8,5], return [0,4,2]

**[题目链接](https://www.lintcode.com/en/problem/count-of-smaller-number/#)**  
**Solution**  
**思路**  
Segment Tree.

**代码**   
```java
class SegmentNode {
    int start;
    int end;
    int value;
    SegmentNode left;
    SegmentNode right;
    public SegmentNode(int start, int end, int value) {
        this.start = start;
        this.end = end;
        this.value = value;
    }
}
public class Solution {
   /**
     * @param A: An integer array
     * @return: The number of element in the array that 
     *          are smaller that the given integer
     */
    public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (queries == null || queries.length == 0) {
            return result;
        }
        SegmentNode root = build(0, 10000);
        for (int v : A) {
            modify(root, v);
        }

        for (int q : queries) {
            result.add(query(root, 0, q - 1));
        }
        return result;
    }
    public SegmentNode build(int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, 0);
        }
        int mid = (start + end) / 2;
        SegmentNode root = new SegmentNode(start, end, 0);
        SegmentNode left = build(start, mid);
        SegmentNode right = build(mid + 1, end);
        root.left = left;
        root.right = right;
        return root;
    }
    public void modify(SegmentNode root, int value) {
        if (root.start == root.end) {
            if (root.start == value) {
                root.value += 1;
            }
            return;
        }
        int mid = (root.start + root.end) / 2;
        if (value <= mid) {
            modify(root.left, value);
        } else {
            modify(root.right, value);
        }
        root.value = root.left.value + root.right.value;
    }
    public int query(SegmentNode root, int start, int end) {
        if (start == root.start && end == root.end || root.start == root.end) {
            return root.value;
        }
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            int left = query(root.left, start, mid);
            int right = query(root.right, mid + 1, end);
            return left + right;
        }
        if (start <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}

```
* * *
#### Count of Smaller Number before itself

**Description**   
>Give you an integer array (index from 0 to n-1, where n is the size of this array, value from 0 to 10000) . For each element Ai in the array, count the number of element before this element Ai is smaller than it and return count number array.
>Example
>For array [1,2,7,8,5], return [0,1,2,3,2]

**[题目链接](https://www.lintcode.com/en/problem/count-of-smaller-number-before-itself/#)**  
**Solution**  
**思路**  
Segment Tree. modify the tree and at the same time, query the interval[0, v - 1].

**代码**   
```java
class SegmentNode {
    int start;
    int end;
    int value;
    SegmentNode left;
    SegmentNode right;
    public SegmentNode(int start, int end, int value) {
        this.start = start;
        this.end = end;
        this.value = value;
    }
}
public class Solution {
   /**
     * @param A: An integer array
     * @return: The number of element in the array that 
     *          are smaller that the given integer
     */
    public ArrayList<Integer> countOfSmallerNumberII(int[] A) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (A == null || A.length == 0) {
            return result;
        }
        SegmentNode root = build(0, 10000);
        for (int v : A) {
            int cnt = 0;
            if (v > 0) {
                cnt = query(root, 0, v - 1);
            }
            modify(root, v);
            result.add(cnt);
        }
        return result;
    }
    public SegmentNode build(int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, 0);
        }
        int mid = (start + end) / 2;
        SegmentNode root = new SegmentNode(start, end, 0);
        SegmentNode left = build(start, mid);
        SegmentNode right = build(mid + 1, end);
        root.left = left;
        root.right = right;
        return root;
    }
    public void modify(SegmentNode root, int value) {
        if (root.start == root.end) {
            if (root.start == value) {
                root.value += 1;
            }
            return;
        }
        int mid = (root.start + root.end) / 2;
        if (value <= mid) {
            modify(root.left, value);
        } else {
            modify(root.right, value);
        }
        root.value = root.left.value + root.right.value;
    }
    public int query(SegmentNode root, int start, int end) {
        if (start == root.start && end == root.end || root.start == root.end) {
            return root.value;
        }
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            int left = query(root.left, start, mid);
            int right = query(root.right, mid + 1, end);
            return left + right;
        }
        if (start <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}

```
* * *

