####Contains duplicates (3)  
 217 Contains Duplicate  
 219 Contains Duplicate I   
 220 Contains Duplicate II  
* * * 
#### 217. Contains Duplicate 

**Description**   
>Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

**[题目链接](https://leetcode.com/problems/contains-duplicate/)**  
**Solution**  
**思路**  
Hashset.  

**代码**   
```java
import java.util.*;
public class Solution {
    public boolean containsDuplicate(int[] nums) {
        //if(nums == null) return false;
        if(nums.length <= 1) {
            return false;
        }
        HashSet hs = new HashSet();
        for(int i = 0; i < nums.length; i++){
            if(hs.contains(nums[i])) {
                return true;
            } else {
                hs.add(nums[i]);
            }
        }
        return false;
    }
}
```
* * *

#### 219. Contains Duplicate II 

**Description**   
>Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.

**[题目链接](https://leetcode.com/problems/contains-duplicate-ii/)**  
**Solution**  
**思路**  
同样是hash。用hashmap存num的index，检验重复值的index只差<= k。

**代码**   
```java
public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        if (nums == null || nums.length == 0 || k < 1) {
            return false;
        }
        
        HashMap<Integer, Integer> hm = new HashMap();
        
        for (int i = 0; i < nums.length; i++) {
            if (hm.containsKey(nums[i]) && i - hm.get(nums[i]) <= k) {
                return true;
            } else {
                hm.put(nums[i], i);
            }
        }
        return false;
    }
}
```
* * *
#### 220. Contains Duplicate III 

**Description**   
> Given an array of integers, find out whether there are two distinct indices i and j in the array such that the difference between nums[i] and nums[j] is at most t and the difference between i and j is at most k.

**[题目链接](https://leetcode.com/problems/contains-duplicate-iii/)**  
**Solution**  
**思路**  
解法一。
虽然看起来与前边两道题不相关，但从第二题我们想到一个大致的方法，就是一边加入一边删除，首先保证了对于当前位置j，与之相比较的数不会超过k个。
如何确保证明前边k个数中有我们要找的值呢。在前k个数中查找小于等于n的最大元素和大于等于n的最小元素，若存在n - max <= t || min - n >= t，则返回true；
TreeSet可以实现这个算法。TreeSet是用红黑树实现的一种平衡二叉树，插入、删除、查询都是logn，因此时间复杂度为n * logk.
对比时要考虑整型溢出问题。
```
if (floor != null && (long)n  - (long)t <= (long)floor ||
	ceiling != null && (long)ceiling <= (long)t + (long)n) {
	return true;
}
```

**代码**   
```java
public class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if (nums == null || nums.length == 0 || k <= 0 || t < 0) {
            return false;
        }   
        TreeSet<Integer> treeSet = new TreeSet<Integer>();
        for (int i = 0; i < nums.length; i++) {
            Integer floor = treeSet.floor(nums[i]);
            Integer ceiling = treeSet.ceiling(nums[i]);
            if (floor != null && (long)nums[i] - (long)floor <= t ||
                ceiling != null && (long)ceiling - (long)nums[i] <= t) {
                return true;
            }
            treeSet.add(nums[i]);
            if (i >= k) {
                treeSet.remove(nums[i - k]);
            }
        }
        return false;
    }
}
```
解法二。
滑动窗口 + bucket 
* * *

