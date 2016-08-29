33 Search in Rotated Sorted Array  
34 Search for a Range  
35 Search Insert Position  
69 Sqrt(x)  
74 Search a 2D Matrix  
81 Search in Rotated Sorted Array II  
153 Find Minimum in Rotated Sorted Array  
154 Find Minimum in Rotated Sorted Array II  
240 Search a 2D Matrix II  

#### 33. Search in Rotated Sorted Array 

__Description__   
Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.
__Solution__  
**思路**  
虽然rotated array局部可能有一个点反序，但整体还是有序的，考虑用binary search。
假设pivot将数组分为前后两个升序部分，mid与start and end的关系有几种情况。
mid > start，mid肯定位于前半个升序部分。
mid < start, mid肯定位于后半个升序部分。
先比较target与end的关系，看target位于哪一个部分。再比较target与mid，mid与end的关系，看舍弃哪一部分。

 
```java
public class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int start = 0;
        int end = nums.length - 1; 
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (target > nums[end]) { //target on start side
                if (nums[mid] < target  && nums[mid] > nums[end]) {
                    start = mid;
                } else {
                    end = mid;
                }
            } else if (target < nums[end]){ // target on end side
                if (nums[mid] > target && nums[mid] < nums[end]) {
                    end = mid;
                } else  {
                    start = mid;
                }
            } else {
                return end;
            }
        }
        if (nums[start] == target) {
            return start;
        } 
        if (nums[end] == target) {
            return end;
        }
        return -1;
    }
}
```

#### 34. Search for a Range 

__Description__   
>Given a sorted array of integers, find the starting and ending position of a given target value.  
Your algorithm's runtime complexity must be in the order of O(log n).  
If the target is not found in the array, return [-1, -1].    
For example,  
Given [5, 7, 7, 8, 8, 10] and target value 8,  
return [3, 4].  

**Solution**  
两次二分搜索。找到第一个target和最后一个的位置。  
```
1 2 3 3 3 4 5 
      ^
	 mid
```
**思路：**    
target 的position 是第一个还是最后一个，取决于当nums[mid] == target时，
	end == mid (first), or  
	start == mid (last)  
__Solution__  

```java
public class Solution {
    public int[] searchRange(int[] nums, int target) {
        // binary search, get the first position of target and get the last position of target
        int[] re = new int[2];
        
        int start = 0;
        int end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] >= target) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (nums[start] == target) {
            re[0] = start;
        } else if (nums[end] == target) {
            re[0] = end;
        } else {
            re[0] = -1;
            re[1] = -1;
            return re;
        }
        start = 0;
        end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] > target) {
                end = mid;
            } else { // if nums[mid] == target, discard left side
                start = mid;
            }
        }
        if (nums[end] == target) {
            re[1] = end;
        } else if (nums[start] == target) {
            re[1] = start;
        } 
        return re;
    }
}
```
#### 35. Search Insert Position 

**Description**   
>Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.  
You may assume no duplicates in the array.  
Here are few examples.   
`[1,3,5,6]`, 5 → 2  
`[1,3,5,6]`, 2 → 1  
`[1,3,5,6]`, 7 → 4  
`[1,3,5,6]`, 0 → 0  


**Solution**  
**思路**  
数组有序，二分搜索。
if nums[mid] == target, return mid;
若找不到target，不叫最后start, target and end的关系。
```java
public class Solution {
    public int searchInsert(int[] nums, int target) {
        // illegal conditions
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int start = 0;  
        int mid = 0;
        int end = nums.length - 1;
        while(end - start > 1) {
            mid = start + (end - start) / 2;
            if (nums[mid] < target) {
                start = mid;
            } else if (nums[mid] > target){
                end = mid;
            } else {
                return mid;
            }
        }
        if  (nums[start] > target) {
            return start;
        } else if (nums[end] > target) {
            return end;
        } else {
            return end + 1;
        }
    }
}
```
#### 69. Sqrt(x) 

**Description**   
Implement `int sqrt(int x)`.  
**Solution**  
**思路**  
二分。find the largest n so that n * n <= x。比较时不能要用除法，n <= x / n。否则会越界。

**代码**
```java
public class Solution {
    public int mySqrt(int x) {
        // find the largest n so that n * n <= x;
        int start = 0;
        int end = x / 2 + 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (mid > x / mid) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (end <= x / end) {
            return end;
        }
        return start;
    }
}
```

####  74. Search a 2D Matrix

__Description__    
>Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
>
>Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
For example,  
Consider the following matrix:
```
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```
Given target = 3, return true.   

__Solution__  
**思路**   
思路1. 两次二分查找。先从最后一列二分，查找first pos >= target。确定行后再这一行二分。    
思路2，将二维数组看做一维数组直接二分。
**代码**  
```java
public class Solution {
    public int getRow(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
        // first col, find last p <= target
        int start = 0;
        int end = m - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (matrix[mid][n - 1] >= target) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (matrix[start][n - 1] >= target) {
            return start;
        }
        if (matrix[end][n - 1] >= target) {
            return end;
        }
        return -1;
    }
    public boolean binarySearch(int[] array, int target) {
        int start = 0;
        int end = array.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (array[mid] < target) {
                start = mid;
            } else if (array[mid] > target){
                end = mid;
            } else {
                return true;
            }
        }
        
        if (array[start] == target || array[end] == target) {
            return true;
        }
        return false;
    }
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 ||
            matrix[0] == null || matrix[0].length == 0) {
            return false;
        }
        int row = getRow(matrix, target);
        if (row == -1) {
            return false;
        }
        return binarySearch(matrix[row], target);
    }
}
```

**思路2**  
```java  
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 ||
            matrix[0] == null || matrix[0].length == 0) {
            return false;
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int len = m * n;
        int start = 0;
        int end = len - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (matrix[mid / n][mid % n] < target) {
                start = mid;
            } else if (matrix[mid / n][mid % n] > target){
                end = mid;
            } else {
                return true;
            }
        }
        return (matrix[start / n][start % n] == target || matrix[end / n][end % n] == target);
        
    }
}
```
#### 81. Search in Rotated Sorted Array II 

__Description__   
>Follow up for "Search in Rotated Sorted Array":  
What if duplicates are allowed?  
Would this affect the run-time complexity? How and why?  
Write a function to determine if a given target is in the array.  


__Solution__ 

**worst is O(n), for example **  
** find 0 in [1, 1, 1, 1, 0, 1, 1, 1] **  
**binary search will not work, because if mid is at plateu, we can not decide to discard left or right**  
思路1， 直接遍历数组。  
思路2，先判断是否是升序，升序直接二分。若不是升序，divide and couquer，先左半部分，找到返回true，找不到再找右边。因此能部分剪枝。    
判断升序与否？nums[start] < nums[end].  
**思路1**  
```java
public class Solution {
    public boolean search(int[] nums, int target) {
        // worst is O(n), for example 
        // find 0 in [1, 1, 1, 1, 0, 1, 1, 1] 
        // binary search will not work, because if mid is at plateu, we can not decide to discard left or right
        if (nums == null || nums.length == 0) {
            return false;
        }
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                return true;
            }
        }
        return false;
    }
}
```
**思路2**  
```java
public class Solution {
    public boolean binarySearch(int[] array, int target, int start, int end) {
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (array[mid] < target) {
                start = mid;
            } else if (array[mid] > target){
                end = mid;
            } else {
                return true;
            }
        }
        return (array[start] == target || array[end] == target);
    }
    public boolean searchHelper(int[] nums, int target, int start, int end) {
        if (start >=  end) {
            return nums[start] == target;
        } 
        if (nums[start] < nums[end]) {
            return binarySearch(nums, target, start, end);
        }
        int mid = start + (end - start) / 2;
        if (searchHelper(nums, target, start, mid)) {
            return true;
        }
        return searchHelper(nums, target, mid + 1, end);
    }
    public boolean search(int[] nums, int target) {
        return searchHelper(nums, target, 0, nums.length - 1);
    }
}
```

#### 153. Find Minimum in Rotated Sorted Array 

__Description__   
>Suppose a sorted array is rotated at some pivot unknown to you beforehand.  
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).    
Find the minimum element.   
You may assume no duplicate exists in the array.  

__Solution__ 

**思路1**  
直接二分查找。mid > end， min在[mid, end], mid < start, min在[start, mid]，else, 数组升序, return start。  
**思路2**  递归。  
**代码**  
**思路1**   
```java
public class Solution {
    public int findMin(int[] nums) {
        
        if (nums.length == 1) {
            return nums[0];
        }
        int start = 0;
        int end = nums.length - 1;
        
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] > nums[end]) {
                start = mid;
            } else if (nums[mid] < nums[start]) {
                end = mid;
            } else {
                return nums[start];
            }
        }
        return Math.min(nums[start], nums[end]);
        
    }
}
```

**思路2**  
```java
public class Solution {
    public int search(int[] nums, int l, int r) {
        if (l >= r) {
            return nums[l];
        }
        if (nums[l] <= nums[r]) {
            return nums[l];
        }
        int mid = l + (r - l) / 2;
        int left = search(nums, l, mid);
        int right = search(nums, mid + 1, r);
        return Math.min(left, right);
    }
    public int findMin(int[] nums) {
        return search(nums, 0, nums.length - 1);
    }
}
```
#### 154. Find Minimum in Rotated Sorted Array II 

__Description__   
>Follow up for "Find Minimum in Rotated Sorted Array":
>What if duplicates are allowed?
>
>Would this affect the run-time complexity? How and why?


__Solution__ 

**思路1**  
首先想到binary search。但因为数组有重复，需要特殊处理。  
要用到递归。 先判断是否升序，升序return start。 
divide to left and right, earch both, return min.

**代码**  
**思路1**  

**思路2**  
```jav
public class Solution {
    public int search(int[] nums, int l, int r) {
        if (l >= r) {
            return nums[l];
        }
        if (nums[l] < nums[r]) {
            return nums[l];
        }
        int mid = l + (r - l) / 2;
        int left = search(nums, l, mid);
        int right = search(nums, mid + 1, r);
        return Math.min(left, right);
    }
    public int findMin(int[] nums) {
        return search(nums, 0, nums.length - 1);
    }
}
```


#### 240. Search a 2D Matrix II 

__Description__   
>Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:  
Integers in each row are sorted in ascending from left to right.  
Integers in each column are sorted in ascending from top to bottom.  
For example,  
Consider the following matrix:   
```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]  
```
>Given target = 5, return true.    
Given target = 20, return false.    



__Solution__   
**思路1**   
对每一行二分，复杂度O(m * logn)。
**思路2**   
从矩阵右上角开始，若当前位置>target，舍弃当前列，因为当前列的所有数都大于target。若当前位置< target，舍弃当前行，因为当前行的所有数都小于target。相等则返回。

**代码**   
**思路1**   
```java 
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        if (matrix[0] == null || matrix[0].length == 0) {
            return false;
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
        int ind = 0;
        while (ind < m) {
            int start = 0;
            int end = n - 1;
            while (start + 1 < end) {
                int mid = start + (end -start) / 2;
                if (matrix[ind][mid] > target) {
                    end = mid;
                } else if (matrix[ind][mid] < target) {
                    start = mid;
                } else {
                    return true;
                }
            }
            if (matrix[ind][start] == target) {
                return true;
            }
            if (matrix[ind][end] == target) {
                return true;
            }
            ind++;
        }
        return false;
    }
}
```
**思路2**  
```java 
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        if (matrix[0] == null || matrix[0].length == 0) {
            return false;
        }
        
        int m = matrix.length;
        int n = matrix[0].length;
        int row = 0;
        int col = n - 1;
        while(row < m && col >= 0) {
            if (matrix[row][col] > target) {
                col--;
            } else if (matrix[row][col] < target) {
                row++;
            } else {
                return true;
            }
        }
        return false;
    }
}
```




