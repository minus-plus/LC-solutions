#LeetCode新题
#### 396. Rotate Function  

**Description**   
>Given an array of integers A and let n to be its length.
>
>Assume Bk to be an array obtained by rotating the array A k positions clock-wise, we define a "rotation function" F on A as follow:
>
>F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1].
>
>Calculate the maximum value of F(0), F(1), ..., F(n-1).
>
>Note:
>n is guaranteed to be less than 105.
>
>Example:
```
A = [4, 3, 2, 6]
F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26
So the maximum value of F(0), F(1), F(2), F(3) is F(3) = 26.
```

**[题目链接](https://leetcode.com/problems/rotate-function/)**  
**Solution**  
**思路**  
数学公式推导。  
第i次旋转， i start from 0:  
value = value - (n - 1) * (A[n - 1 - i]) + (sum - A[n - 1 - i])  
          = value  - n * A[n - 1 - i] + sum  
直接迭代即可。

**代码**   
```java
public class Solution {
    public int maxRotateFunction(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        int n = A.length;
        int sum = sum(A);
        int value = getValue(A);
        int max = value;

        for (int i = 0; i < A.length - 1; i++) {
            value = value - n * A[n - 1 - i] + sum;
            max = Math.max(value, max);
        }
        return max;
    }
    public int getValue(int[] A) {
        int value = 0;
        for (int i = 0; i < A.length; i++) {
            value += i * A[i];
        }
        return value;
    }
    public int sum(int[] A) {
        int sum = 0;
        for (int i : A) {
            sum += i;
        }
        return sum;
    }
}
```
* * *
#### 403. Frog Jump

**Description**   
>A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.
>
>Given a list of stones' positions (in units) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit.
>
>If the frog's last jump was k units, then its next jump must be either k - 1, k, or k + 1 units. Note that the frog can only jump in the forward direction.
>
>Note:
>
>The number of stones is ≥ 2 and is < 1,100.
>Each stone's position will be a non-negative integer < 231.
>The first stone's position is always 0.
>Example 1:
```
[0,1,3,5,6,8,12,17]

There are a total of 8 stones.
The first stone at the 0th unit, second stone at the 1st unit,
third stone at the 3rd unit, and so on...
The last stone at the 17th unit.

Return true. The frog can jump to the last stone by jumping 
1 unit to the 2nd stone, then 2 units to the 3rd stone, then 
2 units to the 4th stone, then 3 units to the 6th stone, 
4 units to the 7th stone, and 5 units to the 8th stone.
```
>Example 2:
```
[0,1,2,3,4,8,9,11]

Return false. There is no way to jump to the last stone as 
the gap between the 5th and 6th stone is too large.
```

**[题目链接](https://leetcode.com/problems/frog-jump/)**  
**Solution**  
**思路**  
1. DFS + memorization, MET, depth too large
2. DP, 学校一个新知识点，二维数组matrix[i]的长度可以是不同的，这样可以节省空间。
3. 迭代法，用hashmap store每个石头可以跳的所有值，在当前石头判断可以reach哪些石头，并用当前石头的jump更新 set of reached stone.  

**代码**   
**DP**   
```java
public class Solution {
    public boolean canCross(int[] stones) {
        if (stones == null || stones.length < 2) {
            return false;
        }
        int maxJump = 0;
        boolean[][] dp = new boolean[stones.length][];
        dp[0] = new boolean[]{true};
        for (int i = 1; i < stones.length; i++) {
            dp[i] = new boolean[maxJump + 2];
            for (int j = 1; j < dp[i].length; j++) {
                int pos = getPos(stones, stones[i] - j, i);
                if (pos >= 0) {
                    if (j - 1 < dp[pos].length && dp[pos][j - 1] ||j < dp[pos].length && dp[pos][j] || j + 1 < dp[pos].length && dp[pos][j + 1]) {
                        dp[i][j] = true;
                        maxJump = Math.max(maxJump, j);
                    }
                }
            }
        }
        
        for (int i = 1; i < dp[stones.length - 1].length; i++) {
            if (dp[stones.length - 1][i]) {
                return true;
            }
        }
        return false;
    }
    
    public int getPos(int[] nums, int val, int index) {
        for (int i = index - 1; i >= 0; i--) {
            if (nums[i] == val) {
                return i;
            }
        }
        return -1;
    }
    
}
```
**Iterative**  
```java
public class Solution {
    public boolean canCross(int[] stones) {
        if (stones == null || stones.length < 2) {
            return false;
        }
        HashMap<Integer, HashSet<Integer>> map = new HashMap<Integer, HashSet<Integer>>();
        for (int i = 0; i < stones.length - 1; i++) {
            map.put(stones[i], new HashSet<Integer>());
        }
        map.get(0).add(1);
        for (int i = 0; i < stones.length - 1; i++) {
            for (int j : map.get(stones[i])) {
                int reach = stones[i] + j;
                if (reach == stones[stones.length - 1]) {
                    //printSet(map, stones);
                    return true;
                }
                HashSet reachSet = map.get(reach); // stones does not contain that stone
                if (reachSet != null) {
                    for (int k = -1; k <= 1; k++) {
                        if (j + k > 0) {
                            map.get(reach).add(j + k);
                        }
                    }
                }
            }
        }
        
        return false;
    }
    public void printSet(HashMap<Integer, HashSet<Integer>> map, int[] stones) {
        for (int i = 0; i < stones.length - 1; i++) {
            for (int j: map.get(stones[i])) {
                System.out.print(j + " ");
            }
            System.out.println();
        }
    }
}
```
* * *
#### 398. Random Pick Index  

**Description**   
>ven an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.
>
>Note:
>The array size can be very large. Solution that uses too much extra space will not pass the judge.
>
>Example: 
```
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);
// pick(3) should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(3);
// pick(1) should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(1);
```

**[题目链接](https://leetcode.com/problems/random-pick-index/)**  
**Solution**  
**思路**  
reservior sampling to simulate equal problity sampling.   

**代码**   
```java
public class Solution {
    public int[] nums;
    public Random rand;
    public Solution(int[] nums) {
        this.nums = nums;
        rand = new Random();
    }
    
    public int pick(int target) {
        int count = 0;
        int re = 0;
        for (int i = 0; i < this.nums.length; i++) {
            if (this.nums[i] == target) {
                count++;
                if (rand.nextInt(count) == 0) {
                    re = i;
                }
            }
        }
        return re;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int param_1 = obj.pick(target);
 */
```
* * *


#### 380. Insert Delete GetRandom O(1)  

**Description**   
>Design a data structure that supports all following operations in average O(1) time.
>
>insert(val): Inserts an item val to the set if not already present.
>remove(val): Removes an item val from the set if present.
>getRandom: Returns a random element from current set of elements. Each element must have the same probability of being returned.
>Example:

```java
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 1 is the only number in the set, getRandom always return 1.
randomSet.getRandom();
```

**[题目链接](https://leetcode.com/problems/insert-delete-getrandom-o1/)**  
**Solution**  
**思路**  
HashMap + ArrayList。
remove时，若删除list非最后一个元素，将最后一个元素复制到删除位置。然后总是删除最后一位。  

**代码**   
```java
public class RandomizedSet {
    private ArrayList<Integer> list;
    private HashMap<Integer, Integer> map;
    private Random rand = new Random();
    private int size;
    
    /** Initialize your data structure here. */
    public RandomizedSet() {
        this.list = new ArrayList<Integer>(); // to store items
        this.map = new HashMap<Integer, Integer>(); // store items
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {
        if (map.containsKey(val)) {
            return false;
        }
        map.put(val, list.size());
        list.add(val);
        return true;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {
        // what if remove the last item of the list
        if (!map.containsKey(val)) {
            return false;
        }
        int ind = map.remove(val);
        int last = list.get(list.size() - 1); 
        if (ind < list.size() - 1) { // if deleted location is not the last
            list.set(ind, last);
            map.put(last, ind);
        }
        list.remove(list.size() - 1); // always remove last element
        return true;
        
    }
    
    /** Get a random element from the set. */
    public int getRandom() {
        int ind = rand.nextInt(list.size());
        return list.get(ind);
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```
* * *
#### 381. Insert Delete GetRandom O(1) - Duplicates allowed  

**Description**   
>Design a data structure that supports all following operations in average O(1) time.
>
>Note: Duplicate elements are allowed.
>insert(val): Inserts an item val to the collection.
>remove(val): Removes an item val from the collection if present.
>getRandom: Returns a random element from current collection of elements. The probability of each >element being returned is linearly related to the number of same value the collection contains.
>Example:

```java
// Init an empty collection.
RandomizedCollection collection = new RandomizedCollection();

// Inserts 1 to the collection. Returns true as the collection did not contain 1.
collection.insert(1);

// Inserts another 1 to the collection. Returns false as the collection contained 1. Collection now contains [1,1].
collection.insert(1);

// Inserts 2 to the collection, returns true. Collection now contains [1,1,2].
collection.insert(2);

// getRandom should return 1 with the probability 2/3, and returns 2 with the probability 1/3.
collection.getRandom();

// Removes 1 from the collection, returns true. Collection now contains [1,2].
collection.remove(1);

// getRandom should return 1 and 2 both equally likely.
collection.getRandom();
```

**[题目链接](https://leetcode.com/problems/insert-delete-getrandom-o1-duplicates-allowed/)**  
**Solution**  
**思路**  
同样是HashMap + ArrayList。  
难点是如何处理重复值。解决方法是将重复值对应的index放入set中。   
删除时，先查找set，从中删除一个index，若index不是最后一位，挖坑填入最后一个元素。总是将最后一位删除。  若set删除一个元素之后size == 0，map.remove(val);

**代码**   
```java
public class RandomizedCollection {

    /** Initialize your data structure here. */
    private ArrayList<Integer> list;
    private HashMap<Integer, Set<Integer>> map;
    private Random rand;
    public RandomizedCollection() {
        list = new ArrayList<Integer>();
        map = new HashMap<Integer, Set<Integer>>();
        rand = new Random();
    }
    
    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    public boolean insert(int val) {
        boolean contains = map.containsKey(val);
        if (!contains) {
            map.put(val, new HashSet<Integer>());
        }
        map.get(val).add(list.size());
        list.add(val);
        return !contains;
    }
    
    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    public boolean remove(int val) {
        if (!map.containsKey(val)) {
            return false;
        } else {
            Set<Integer> set = map.get(val);
            int removedIndex = map.get(val).iterator().next();
            set.remove(removedIndex);
            int lastElement = list.get(list.size() - 1);
            
            if (removedIndex < list.size() - 1) {
                // fill last to removedIndex
                list.set(removedIndex, lastElement);
                map.get(lastElement).remove(list.size() - 1);
                map.get(lastElement).add(removedIndex);
            }
            
            if (set.size() == 0) {
                map.remove(val);
            }
            list.remove(list.size() - 1);
            return true;
        }
        
    }
    
    /** Get a random element from the collection. */
    public int getRandom() {
        return list.get(rand.nextInt(list.size()));
    }
  
}

/**
 * Your RandomizedCollection object will be instantiated and called as such:
 * RandomizedCollection obj = new RandomizedCollection();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```
* * *

