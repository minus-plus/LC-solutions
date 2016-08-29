# K sum 系列
```
1. Two Sum
15. 3Sum
16. 3Sum Closest
18. 4Sum  
```

---
#### 1. Two Sum

__Description__   
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
__Solution__  
思路：  
HashMap记录`<target - nums[i], i>`，若当前nums[i]在HashMap中，则结果找到，else put(target - nums[i], i);  
复杂度：O(n) time, O(n) space。暴力解法O(n^2)  
```java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> hm = new HashMap();
        int[] re = new int[2];
        for (int i = 0; i < nums.length; i++) {
            if (hm.containsKey(nums[i])) {// target found
                re[0] = hm.get(nums[i]);
                re[1] = i;
            } else {
                hm.put(target - nums[i], i);
            }
        }
        return re;
    }
}
```
#### 15. 3Sum 

__Description__   
Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.
Note: The solution set must not contain duplicate triplets.

For example, given array S = [-1, 0, 1, 2, -1, -4],

A solution set is:
```
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```
__Solution__  
__思路：__  
sort数组，枚举位置i，然后双指针法two sum，双指针复杂度O(n)。复杂度O(n^2)；


```java
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (nums == null || nums.length < 3) {
            return result;
        }
        int len = nums.length;
        Arrays.sort(nums);
        for (int i = 0; i < len - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int l = i + 1;
            int r = len - 1;
            while (l < r) {
                if (l > i + 1 && nums[l] == nums[l - 1]) {
                    l++;
                    continue;
                }
                if (r < len - 1 && nums[r] == nums[r + 1]) {
                    r--;
                    continue;
                }
                if (nums[l] + nums[r] + nums[i] == 0) {
                    ArrayList<Integer> list = new ArrayList<Integer>();
                    list.add(nums[i]);
                    list.add(nums[l]);
                    list.add(nums[r]);
                    result.add(list);
                    l++;
                    r--;
                } else if (nums[l] + nums[r] + nums[i] < 0) {
                    l++;
                } else {
                    r--;
                }
            }
        }
        return result;
    }
}
```
#### 16. 3Sum Closest

__Description__  
Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

    For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
__Solution__   
__思路:__   
类似于3 sum问题，但不用去重。首先sort array，枚举位置i，用双指针法遍历i + 1 ~ n。计算每步sum与target的差值，并记录diff最小时的sum值。复杂度O(n^2)
```java
public class Solution {
    public int threeSumClosest(int[] nums, int target) {
        if (nums == null || nums.length < 3) {
            return 0;
        }
        int min = Integer.MAX_VALUE;
        int result = 0;
        Arrays.sort(nums);
        int len = nums.length;
        for (int i = 0; i < len - 2; i++) {
            int l = i + 1;
            int r = len - 1;
            while (l < r) {
                int sum = nums[i] + nums[l] + nums[r];
                int sub = Math.abs(sum - target);
                if (sub < min) {
                    result = sum;
                    min = Math.min(min, sub);
                }
                if (sum < target) {
                    l++;
                } else if (sum > target) {
                    r--;
                } else {
                    return target;
                }
            }
        }
        return result;
    }
}
```
#### 18. 4Sum 

__Description__   
Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note: The solution set must not contain duplicate quadruplets.

For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
```
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```
__Solution__  

__思路：__  
先对数组排序，枚举位置i和j，然后用双指针法遍历右侧部分，注意去重，复杂度O(n^3)。
__code__  
```java
public class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList();
        if (nums == null || nums.length < 4) {
            return result;
        }
        Arrays.sort(nums);
        int len = nums.length;
        for (int i = 0; i < len - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < len - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                int l = j + 1;
                int r = len - 1;
                while (l < r) {
                    if (l > j + 1 && nums[l] == nums[l - 1]) {
                        l++;
                        continue;
                    }
                    if (r < len - 1 && nums[r] == nums[r + 1]) {
                        r--;
                        continue;
                    }
                    int sum = nums[i] + nums[j] + nums[l] + nums[r];
                    if (sum < target) {
                        l++;
                    } else if (sum > target) {
                        r--;
                    } else {
                        List<Integer> list = new ArrayList<Integer>();
                        list.add(nums[i]);
                        list.add(nums[j]);
                        list.add(nums[l]);
                        list.add(nums[r]);
                        result.add(list);
                        l++;
                        r--;
                    }
                }
            }
        }
        return result;
    }
}
```
优化
```java
public class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList();
        if (nums == null || nums.length < 4) {
            return result;
        }
        Arrays.sort(nums);
        int len = nums.length;
        if (nums[0] * 4 > target || nums[len - 1] * 4 < target) {
            return result;
        }
        for (int i = 0; i < len - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < len - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                if (nums[i] + nums[j] + nums[j + 1] * 2 > target || nums[i] + nums[j] + nums[len - 1] * 2 < target) {
                    continue;
                }
                int l = j + 1;
                int r = len - 1;
                while (l < r) {
                    if (l > j + 1 && nums[l] == nums[l - 1]) {
                        l++;
                        continue;
                    }
                    if (r < len - 1 && nums[r] == nums[r + 1]) {
                        r--;
                        continue;
                    }
                    int sum = nums[i] + nums[j] + nums[l] + nums[r];
                    if (sum < target) {
                        l++;
                    } else if (sum > target) {
                        r--;
                    } else {
                        List<Integer> list = new ArrayList<Integer>();
                        list.add(nums[i]);
                        list.add(nums[j]);
                        list.add(nums[l]);
                        list.add(nums[r]);
                        result.add(list);
                        l++;
                        r--;
                    }
                }
            }
        }
        return result;
    }
}

```
