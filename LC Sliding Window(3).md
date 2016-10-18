Sliding Window (3)  


76 Minimum Window Substring    
209 Minimum Size Subarray Sum  
405 Maximum Size Subarray Sum Equals k  

#### 76. Minimum Window Substring  

**Description**   
>Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).
>
>For example,
>S = "ADOBECODEBANC"
T = "ABC"
>Minimum window is "BANC".

**[题目链接](https://leetcode.com/problems/minimum-window-substring/)**  
**Solution**  
**思路**  
two pointers经典题目。对于pointer i, pointer j++直到找到一个match，更新min。然后i++，如此循环。  

**代码**   
```java
public class Solution {
    public String minWindow(String s, String t) {
        // corner case 
        int[] sh = new int[256];
        int[] th = new int[256];
        for (int i = 0; i < t.length(); i++) {
            th[t.charAt(i)] += 1;
        }
        int min = Integer.MAX_VALUE;
        int j = 0;
        String str = "";
        for (int i = 0; i < s.length(); i++) {
            while (j < s.length() && !validate(sh, th)) {
                sh[s.charAt(j)] += 1;
                j++;
            }
            if (validate(sh, th)) {
                
                if (min > j - i) {
                    min = j - i;
                    str = s.substring(i, j);
                }
            }
            sh[s.charAt(i)] -= 1;
        }
        return str;
    }
    public boolean validate(int[] sh, int[] th) {
        for (int i = 0; i < sh.length; i++) {
            if (sh[i] < th[i]) {
                return false;
            }
        }
        return true;
    }
}
```
* * *
#### 209. Minimum Size Subarray Sum  

**Description**   
>Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return 0 instead.
>
>For example, given the array [2,3,1,2,4,3] and s = 7,
the subarray [4,3] has the minimal length under the problem constraint.

**[题目链接](https://leetcode.com/problems/minimum-size-subarray-sum/)**  
**Solution**  
**思路**  
典型的two pointers题。interation  j++直到找到一个match。更新min。i++，条件又不被满足，开始下一循环。因此这一类题都有一个固定模式。

**代码**   
```java
public class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int m = nums.length;
        int min = Integer.MAX_VALUE;
        int sum = 0;
        int j = 0;
        for (int i = 0; i < nums.length; i++) {
            while (j < nums.length && sum < s) {
                sum += nums[j];
                j++;
            }
            if (sum >= s) {
                min=  Math.min(min, j - i);
            }
            sum -= nums[i];
        }
        return min == Integer.MAX_VALUE ?  0 : min;
    }
}
```
* * *

####405. Maximum Size Subarray Sum Equals k

**Description**   
>Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

>Example 1:
>Given nums = [1, -1, 5, -2, 3], k = 3,
>return 4. (because the subarray [1, -1, 5, -2] sums to 3 and is the longest)

>Example 2:
>Given nums = [-2, -1, 2, 1], k = 1,
>return 2. (because the subarray [-1, 2] sums to 1 and is the longest)

>Follow Up:
>Can you do it in O(n) time?

**[题目链接](void)**  
**Solution**  
**思路**  
类比，arr[j] - arr[i] == k, j - i的最大值。  
暴力解法，枚举i, j, O(n^2).  
空间换时间，只枚举i，hash记录i，hash查找arr[i] - k是否存在，若存在，查找index， max = max(max, i - index)。  

**代码**   
```java
import java.util.*;
public class Solution405 {
    public int maxSubArrayLen(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        HashMap<Integer, Integer> hm = new HashMap<Integer, Integer>();
        int sum = 0;
        int max = 0;
        hm.put(0, -1);
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (hm.containsKey(sum - k)) {
                max = Math.max(max, i - hm.get(sum - k));
            } 
            if (!hm.containsKey(sum)) {
                hm.put(sum, i);
            }
        }
        return max;
    }
    public static void main(String[] args) {
        Solution405 s = new Solution405();
        int[] nums = new int[]{1, -1, 5, -2, 3};
        int re = s.maxSubArrayLen(nums, 3);
        System.out.println("result is " + re + ", should be " + 4);
        System.out.println("result is " + s.maxSubArrayLen(nums, 6) + ", should be " + 5);
    }
}
```
* * *
