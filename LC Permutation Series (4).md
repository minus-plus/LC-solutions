>31 Next Permutation  
>46 Permutations  
>47 Permutations II  
>60 Permutation Sequence  

---

#### 31. Next Permutation 

**Description**   
>Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.  
If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).  
The replacement must be in-place, do not allocate extra memory.  
Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.   
`1,2,3` → `1,3,2`
`3,2,1` → `1,2,3`
`1,1,5` → `1,5,1`  

**Solution**  
**思路**  
WikiPedia  
> 1 Find the largest index k such that a[k] < a[k + 1]. If no such index exists, the permutation is the last permutation.  
2 Find the largest index l such that a[k] < a[l]. Since k+1 is such an index, l is well defined and satisfies k < l.  
3 Swap a[k] with a[l].  
4 Reverse the sequence from a[k+1] up to and including the last element a[n].  

找到最大的index k, which nums[k] < nums[k + 1]， 找不到，则是最后一个排列。  
从k + 1开始往右，找到最大的l，which nums[k] < nums[l]，交换k和l。  
将k + 1 ~ n部分逆序。
```java
public class Solution {
    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    public void reverse(int[] nums, int i, int j) {
        while (i < j) {
            swap(nums, i, j);
            i++;
            j--;
        }
    }
    public void nextPermutation(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return;
        }
        // find partition number k
        int k;
        for (k = nums.length - 2; k >= 0; k--) {
            if (nums[k] < nums[k + 1]) {
                break;
            }
        }
        if (k == -1) {
            reverse(nums, 0, nums.length - 1);
            return;
        }
        // find swap number l
        int l;
        for (l = nums.length - 1; l >= k + 1; l--) {
            if (nums[k] < nums[l]) {
                break;
            }
        }
        // swap
        swap(nums, k, l);
        // reverse k + 1 ~ n
        reverse(nums, k + 1, nums.length - 1);
    }
}
```
#### 46. Permutations 

**Description**   
>Given a collection of distinct numbers, return all possible permutations.
>
>For example,  
`[1,2,3]` have the following permutations:
```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```  

**Solution**  
**思路**  
全排列，DFS大法好。  
也可以用nextPermutation来做。不放代码了。  
```java
public class Solution {
    public void dfs(int[] nums, List<List<Integer>> result, List<Integer> path) {
        if (path.size() == nums.length) {
            result.add(new ArrayList<Integer>(path));
            return;
        }
        for (int n : nums) {
            if (!path.contains(n)) {
                path.add(n);
                dfs(nums, result, path);
                path.remove(path.size() - 1);
            }
        }
    }
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList();
        if (nums == null || nums.length == 0) {
            return result;
        }
        List<Integer> path = new ArrayList();
        dfs(nums, result, path);
        return result;
    }
}
```
#### 47. Permutations II 

**Description**   
>Given a collection of numbers that might contain duplicates, return all possible unique permutations.

>For example,
`[1,1,2]` have the following unique permutations:  
```
[ 
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

**Solution**  
**思路**  
DFS。但要去重复。需要将数组排序。用`i == 0 || nums[i] != nums[i - 1]`判断是否重复。因为有重复，因此不能用path.contians(nums[i]) 来判断该值是否已经加入。需要维护一个map来标记当i是否已经加入。 
`leaf state`: no numbers available to be added. 
`goal state`: `path.size() == nums.length`, get a permutation  
```java
public class Solution {
    public void dfs(int[] nums, List<List<Integer>> result, List<Integer> path, int[] visited) {
        // leaf state, no numbers available
        // goal state, get a permuatation
        if (path.size() == nums.length) {
            result.add(new ArrayList<Integer>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (visited[i] == 0 && (i == 0 || visited[i - 1] == 1 || nums[i] != nums[i - 1])) {
                //当前值没有被访问过，且没有与同一层的其他元素重复。
                path.add(nums[i]);
                visited[i] = 1;
                dfs(nums, result, path, visited);
                path.remove(path.size() - 1);
                visited[i] = 0;
            } // else do nothing, jump over i
        }
    }
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList();
        List<Integer> path = new ArrayList();
        int[] visited = new int[nums.length];
        dfs(nums, result, path, visited);
        return result;
    }
}
```
#### 47. Permutations II 

**Description**   
>Given a collection of numbers that might contain duplicates, return all possible unique permutations.

>For example,
`[1,1,2]` have the following unique permutations:  
```
[ 
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

**Solution**  
**思路1**  
DFS。但要去重复。需要将数组排序。用`i == 0 || nums[i] != nums[i - 1]`判断是否重复。因为有重复，因此不能用path.contians(nums[i]) 来判断该值是否已经加入。需要维护一个map来标记当i是否已经加入。 
`leaf state`: no numbers available to be added. 
`goal state`: `path.size() == nums.length`, get a permutation  

**思路2**  
用next permutation，求数组长的下一个permutation。
修改一下nextPermutation()，当当前处于最后一个permutation时，返回false，停止计算下一个permutation。  
```java
public class Solution {
    public void dfs(int[] nums, List<List<Integer>> result, List<Integer> path, int[] visited) {
        // leaf state, no numbers available
        // goal state, get a permuatation
        if (path.size() == nums.length) {
            result.add(new ArrayList<Integer>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (visited[i] == 0 && (i == 0 || visited[i - 1] == 1 || nums[i] != nums[i - 1])) {
                //当前值没有被访问过，且没有与同一层的其他元素重复。
                path.add(nums[i]);
                visited[i] = 1;
                dfs(nums, result, path, visited);
                path.remove(path.size() - 1);
                visited[i] = 0;
            } // else do nothing, jump over i
        }
    }
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList();
        List<Integer> path = new ArrayList();
        int[] visited = new int[nums.length];
        dfs(nums, result, path, visited);
        return result;
    }
}
```

**思路2**  
```java
public class Solution {
    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    public void reverse(int[] nums, int i, int j) {
        while (i < j) {
            swap(nums, i, j);
            i++;
            j--;
        }
    }
    public boolean nextPermutation(int[] nums) {
        int k;
        for (k = nums.length - 2; k >= 0; k--) {
            if (nums[k] < nums[k + 1]) {
                break;
            }
        }
        if (k == -1) {
            return false;
        }
        int l;
        for (l = nums.length - 1; l >= k + 1; l--) {
            if (nums[k] < nums[l]) {
                break;
            }
        }
        swap(nums, k, l);
        reverse(nums, k + 1, nums.length - 1);
        return true;
    }
    public List<Integer> arrayToList(int[] nums) {
        List<Integer> list = new ArrayList<Integer>();
        for (int n : nums) {
            list.add(n);
        }
        return list;
    }
    public List<List<Integer>> permuteUnique(int[] nums) {
        
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (nums == null || nums.length == 0) {
            return result;
        }
        int[] copy = Arrays.copyOf(nums, nums.length);
        Arrays.sort(copy);
        result.add(arrayToList(copy));
        while (nextPermutation(copy)) {
            result.add(arrayToList(copy));
        }
        return result;
    }
}
```
#### 60. Permutation Sequence 

**Description**   
>The set [1,2,3,…,n] contains a total of n! unique permutations.
>
>By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):
```
"123"
"132"
"213"
"231"
"312"
"321"
```
Given n and k, return the kth permutation sequence.  

**Solution**  
**思路**  
直接计算出第`k`个排列。  
首先。对于`n`个数，共有`(n - 1)!`种排列。
我们从左到右按升序先确定n - i个数，则对n - i位置的每一个可选数都对应` (i - 1)!`中排列。
需要维护一个`used` map记录哪些数已经用过。
 `num = k / (i - 1)! +1`，`num`是指要再`used`中找第`num`个没有用过的数。
```java
public class Solution {
    public String getPermutation(int n, int k) {
        int[] used = new int[n];
        StringBuffer sb = new StringBuffer();
        int factor = 1;
        for (int i = 1; i < n; i++) {
            factor *= i;
        }
        k = k - 1;
        
        for (int i = 0; i < n; i++) {
            int count = k / factor;
            int j;
            for (j = 0; j < n; j++) {
                if (used[j] == 0) {
                    if (count == 0) {
                        used[j] = 1;
                        sb.append( j+ 1);
                        break;
                    }
                    count--;
                }
            }
            k = k % factor;
            if (i < n - 1) {
                factor = factor / (n - 1 - i);
            }
            
        }
        return sb.toString();
    }
}
```

```java
public class Solution {
    public int findNumber(int[] used, int k) {
        for (int i = 1; i< used.length; i++) {
            if (used[i] == 0) {
                k--;
            }
            if (k == 0) {
                return i;
            }
        }
        return 0;
    }
    public String getPermutation(int n, int k) {
        int[] used = new int[n + 1];
        StringBuffer sb = new StringBuffer();
        int factor = 1;
        for (int i = 1; i < n; i++) {
            factor *= i;
        }
        k = k - 1;
        
        for (int i = n; i >= 1; i--) {
            int count = k / factor + 1;
            int number = findNumber(used, count);
            used[number] = 1;
            sb.append(number);
            k = k % factor;
            if (i > 1) {
                factor = factor / (i - 1);
            }
        }
        return sb.toString();
    }
}
```




