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
* * *
