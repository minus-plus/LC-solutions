121 Best Time to Buy and Sell Stock  
122 Best Time to Buy and Sell Stock II  
123 Best Time to Buy and Sell Stock III  
188 Best Time to Buy and Sell Stock IV  

* * *   
#### 121. Best Time to Buy and Sell Stock 

__Description__   
>Say you have an array for which the ith element is the price of a given stock on day i.
>
>If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.
>
>Example 1:
Input: [7, 1, 5, 3, 6, 4]
Output: 5
>
>max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
>Example 2:
>Input: [7, 6, 4, 3, 1]
>Output: 0
>
>In this case, no transaction is done, i.e. max profit = 0.  

**[题目链接](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)**

__Solution__  
**思路**  
求买进和卖出的最大差价。注意交易可以为空，最大利润可以为零。用动归记录i位置（包含）之前的min，prices[i] - min就当前i的最大利润，求max即可。

**代码**  
```java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < prices.length; i++) {
            min = Math.min(min, prices[i]);
            max = Math.max(max, prices[i] - min);
        }
        return max;
    }
}
```
* * * 

#### 122. Best Time to Buy and Sell Stock II 

**Description**   
>Say you have an array for which the ith element is the price of a given stock on day i.
>
>Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

**[题目链接](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)**    


**Solution**   
**思路**  
同样是要求低价卖高价卖，题目要求可以无限多次买卖。因此可转化为求取序列连续上升的高度和是多少。
思路1，if (p[i + 1] > p[i]) sum += p[i + 1] - p[i]  
思路2，if (i is max and max > min) sum += max - min  
**代码**   
```java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) {
            return 0;
        }
        int profits = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) {
                profits += prices[i] - prices[i - 1];
            }
        }
        return profits;
    }
}
```
**代码2**  
```java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) {
            return 0;
        }
        int profits = 0;
        int min = prices[0];
        int max = prices[0];
        for (int i = 1; i < prices.length; i++) {
            min = Math.min(min, prices[i]);
            if (prices[i] > prices[i - 1] && (i == prices.length - 1 || prices[i] >= prices[i + 1])) {
                // max
                profits += prices[i] - min;
                min = prices[i]; // start over
            }
        }
        return profits;
    }
}
```
* * *
#### 123. Best Time to Buy and Sell Stock III 

__Description__   
>Say you have an array for which the ith element is the price of a given stock on day i.
>
>Design an algorithm to find the maximum profit. You may complete at most two transactions.
>
>Note:
>You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).  

**[题目链接](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)**

__Solution__  
**思路** 
首先，应将数组分割为左右两个部分，且左右两部分size均可为0；  
然后，对左边每个位置i，求以i结尾的序列的最大利润，可为0；对右边每个位置i求自右向左，以i结尾的最小利润；  
然后，对每个位置i, 0 <= i >= length - 1, max = Math.max(max, left[i] - right[i + 1]);  
最后，因为left[length - 1]没有参与计算，此位置对应左侧为全部，右侧size为0的情况，return Math.max(max, left[length - 1]);  
**通用解法见188题**  

```java
public class Solution {
    public int[] maxLeftProfit(int[] prices) {
        int[] re = new int[prices.length];
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < prices.length; i++) {
            min = Math.min(min, prices[i]);
            max = Math.max(max, prices[i] - min);
            re[i] = max;
        }
        return re;
    }
    public int[] minRightProfit(int[] prices) {
        int[] re = new int[prices.length];
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for (int i = prices.length - 1; i >= 0; i--) {
            max = Math.max(max, prices[i]);
            min = Math.min(min, prices[i] - max);
            re[i] = min;
        }
        return re;
    }
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int[] left = maxLeftProfit(prices);
        int[] right = minRightProfit(prices);
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < prices.length - 1; i++) {
            max = Math.max(max, left[i] - right[i + 1]);
        }
        return Math.max(max, left[prices.length - 1]);
    }
}
```
* * * 

#### 188. Best Time to Buy and Sell Stock IV 

**Description**   
>Say you have an array for which the ith element is the price of a given stock on day i.
>
>Design an algorithm to find the maximum profit. You may complete at most k transactions.
>
>Note:
>You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

**[题目链接](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)**    

**Solution**   
**思路**  
DP。  
state: dp[i][j], maxProfit of <= i transactions and end with j.  
function: dp[i][j] = dp[i][j - 1]), not selling at j  
               dp[i][j] = dp[i - 1][j - 1] + prices[j] + (localMax `before` j), if selling at j  
							 取中间的最大值。  
							 (localMax `at` j) = dp[i - 1][j - 1] - prices[j] (buy at j)  
init: array, no need to init  
answer: dp[i][len - 1];  

**代码**   
```java
public class Solution {
    public int maxProfit(int k, int[] prices) {
        if (k == 0 || prices == null || prices.length == 0) {
            return 0;
        }
        if (k >= prices.length / 2) {
            return maxProfit2(prices);
        }
        // dp[i][j], maxProfit of <= i transactions end with j.
        int[][] dp = new int[k + 1][prices.length]; 
        // init, array, no need to init
        for (int i = 1; i <= k; i++) {
            int localMax = -prices[0];
            for (int j = 1; j < prices.length; j++) {
                dp[i][j] = Math.max(dp[i][j - 1], localMax + prices[j]);
                // localMax is maxProfit of <= i - 1 transactions and 1 buying <= j - 1
                localMax = Math.max(localMax, -prices[j] + dp[i - 1][j - 1]);
            }
        }
        return dp[k][prices.length - 1];
    }
    public int maxProfit2(int[] prices) {
        int sum = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) {
                sum += prices[i] - prices[i - 1];
            }
        }
        return sum;
    }
}
```
* * *

