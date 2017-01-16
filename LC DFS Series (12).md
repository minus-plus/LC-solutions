39 Combination Sum  
40 Combination Sum II  
51 N-Queens  
52 N-Queens II  
77 Combinations  
78 Subsets  
79 Word Search  
90 Subsets II  
200 Number of Islands  
216 Combination Sum III  
241 Different Ways to Add Parentheses  
282 Expression Add Operators  

---
#### 39. Combination Sum 

**Description**   
>Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.  
The same repeated number may be chosen from C unlimited number of times.  
Note:  
All numbers (including target) will be positive integers.  
The solution set must not contain duplicate combinations.  
For example, given candidate set [2, 3, 6, 7] and target 7,   
A solution set is:   
```
[
  [7],
  [2, 2, 3]
]
``` 

**Solution**  
**思路**  
dfs + pos。 
题中规定每个数可以被添加无限多次，且数组中不含重复值，可以用记录下一个可用pos的方法避免重复添加已经加入的数。  
```java
public class Solution { 
    public void dfs(int[] candidates, List<List<Integer>> result, List<Integer> path, int target, int pos, int sum) {
        // leaf node, means should stop digging
        if (sum >= target) {
            if (sum == target) {
                result.add(new ArrayList<Integer>(path));
            }
            return;
        }
        for (int i = pos; i < candidates.length; i++) {
            if (sum + candidates[i] <= target) {
                path.add(candidates[i]);
                dfs(candidates, result, path, target, i, sum + candidates[i]);
                path.remove(path.size() - 1);
            } else {
				break;
			}
            
        }
    }
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (candidates == null || candidates.length == 0) {
            return result;
        }
        List<Integer> path = new ArrayList<Integer>();
        dfs(candidates, result, path, target, 0, 0);
        return result;
    }
}
```

---
#### 40. Combination Sum II 

**Description**   
>Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.  
Each number in C may only be used once in the combination.    
Note: 
All numbers (including target) will be positive integers.  
The solution set must not contain duplicate combinations.  
For example, given candidate set `[10, 1, 2, 7, 6, 1, 5]` and target `8`,   
```
A solution set is: 
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```  

**Solution**    
**思路**   
与39题类似。有两点不同。  
1. 每个数只能用一次
2. 不能有重复的combination。  
解决方法： dfs + pos + sort。在每层栈空间，`i != pos && candidates[i] == candidates[i - 1]`则跳过，避免重复的combination。下层从i + 1开始添加，确保每个数只用1次。  

**代码**  
```java
public class Solution {
     public void dfs(int[] candidates, List<List<Integer>> result, List<Integer> path, int target, int pos, int sum) {
        // leaf node, means should stop digging
        if (sum >= target) {
            if (sum == target) {
                result.add(new ArrayList<Integer>(path));
            }
            return;
        }
        for (int i = pos; i < candidates.length; i++) {
            if (i != pos && candidates[i] == candidates[i - 1]) { // ingnor duplicates in current level
                continue;
            }
            if (sum + candidates[i] <= target) {
                path.add(candidates[i]);
                // by i + 1, each number use only once, avoid to add duplicates in next level
                dfs(candidates, result, path, target, i + 1, sum + candidates[i]); 
                path.remove(path.size() - 1);
            }
            
        }
    }
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        List<Integer> path = new ArrayList<Integer>();
        Arrays.sort(candidates);
        dfs(candidates, result, path, target, 0, 0);
        return result;
        
    }
}
```
---
#### 51. N-Queens 

**Description**   
>The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.
>Given an integer n, return all distinct solutions to the n-queens puzzle.  
![eight queens](http://www.leetcode.com/wp-content/uploads/2012/03/8-queens.png)

>Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.
>
>For example,
>There exist two distinct solutions to the 4-queens puzzle:


```
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```


**Solution**  
**思路**  

分解一下：
1. dfs过程，需要一个数组记录已经放置了哪些位置，一个place数组搞定。
2. leaf and goal node, 已经放置了n个位置。current == n搞定。
3. goal node，将place数组转换为list of string，并将list添加到result。toList方法搞定。
4. inner node，判断next available是否不与之前所有已生成的位置共线。check方法搞定。


**代码**  
```java
public class Solution {
    public boolean check(int[] places, int current, int j) {
        for (int i = 0; i < current; i++) {
            if (places[i] == j || 
                Math.abs(places[i] - j) == current - i) {
                return false;
            }
        }
        return true;
    }
    public List<String> toList(int[] places, char[]qs) {
        List<String> list = new ArrayList();
        for (int p: places) {
            qs[p] = 'Q';
            String s = new String(qs);
            list.add(s);
            qs[p] = '.';
        }
        return list;
    }
    public void dfs(int n, List<List<String>> result, int[] places, int current, char[] qs) {
        if (current == n) {
            result.add(toList(places, qs));
            return;
        }

        for (int i = 0; i < n; i++) {
            if (check(places, current, i)) {
                places[current] = i; // like stack
                dfs(n, result, places, current + 1, qs);
            }
        }
    }
    public List<List<String>> solveNQueens(int n) {
        char[] qs = new char[n];
        for (int i = 0; i < n; i++) { // used to generate row
            qs[i] = '.';
        }
        List<List<String>> result = new ArrayList();
        int[] places = new int[n];
        int current = 0;//next place to be set
        dfs(n, result, places, current, qs);
        return result;
    }
}
```
---
#### 52. N-Queens II 

**Description**   
>Follow up for N-Queens problem.
>
>Now, instead outputting board configurations, return the total number of distinct solutions.


**Solution**  
**思路**  
与51题相同，但不需要将所有结果提取，只需要计数。    
```java
public class Solution {
    public boolean check(int[] places, int current, int j) {
        for (int i = 0; i < current; i++) {
            if (places[i] == j || 
                Math.abs(places[i] - j) == current - i) {
                return false;
            }
        }
        return true;
    }
    public void dfs(int n, int[] places, int current, int[] count) {
        if (current == n) {
            count[0] += 1;
            return;
        }
        for (int i = 0; i < n; i++) {
            if (check(places, current, i)) {
                places[current] = i; // like stack
                dfs(n, places, current + 1, count);
            }
        }
    }
    public int totalNQueens(int n) {
        int[] count = new int[1];
        int[] places = new int[n];
        dfs(n, places, 0, count);
        return count[0];
    }
}
```
---
#### 77. Combinations 

**Description**   
>Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.  
>For example,  
>If n = 4 and k = 2, a solution is:  
```
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**Solution**  
**思路**  
dfs，candidates本身有序。   
**代码**  
```java
public class Solution {
    public void dfs(int n, int k, List<List<Integer>> result, List<Integer> path, int current) {
        // leaf and goal node
        if (path.size() == k) {
            result.add(new ArrayList<Integer>(path));
            return;
        }
        // inner node, go to next choice
        for (int i = current; i <= n; i++) {
            path.add(i);
            dfs(n, k, result, path, i + 1);
            path.remove(path.size() - 1);
        }
    }
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList();
        List<Integer> path = new ArrayList();
        dfs(n, k, result, path, 1);
        return result;
    }
}
```
---
#### 78. Subsets 

__Description__   
Given a set of distinct integers, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,3], a solution is:
```

[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
__Solution__    
**思路**  
思路1，首先想到DFS，但leaf node是什么呢，因为要找到所有可能的情况，意味着每个数本身和空集也是leaf node。结论就是所有的node都是leaf node!  
思路2，借鉴来的，bit manipulation，总共有2^n个subset，比如第5个subset，对应二级制5为101，第1位和第3位为1，则选第一个和第三个数。  
**代码**  
__DFS__  
```java
public class Solution {
    public void dfs(int[] nums, int pos, List<List<Integer>> result, List<Integer> path) {
        //all node is leaf node, but do not return
        result.add(new ArrayList(path));
        // go to next choice
        for (int i = pos; i < nums.length; i++) { 
            path.add(nums[i]);
            dfs(nums, i + 1, result, path); // go to next pos, avoid duplicates
            path.remove(path.size() - 1);
        }
    }
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList();
        if (nums == null || nums.length == 0) {
            return result;
        }
        List<Integer> path = new ArrayList();
        dfs(nums, 0, result, path);
        return result;
    }
}
```
__Bit manipulation__    
```java
public class Solution {
    public void backtracking(int[] nums, int k, List<List<Integer>> ll, List<Integer> l) {
        if (k == 0) {
            ll.add(l);
            return;
        }
        int end = l.size() > 0 ? l.get(l.size() - 1) : Integer.MIN_VALUE;
        for (int n : nums) {
            if (n > end) {
                ArrayList<Integer> newList = new ArrayList();
                newList.addAll(l);
                newList.add(n);
                backtracking(nums, k - 1, ll, newList);
            }
        }
    }
    public List<List<Integer>> subsets(int[] nums) {
        // all combinations of k = 1, k = 2, k =3, ... k = nums.length
        List<List<Integer>> ll = new ArrayList();
        ll.add(new ArrayList());
        for (int k = 1; k <= nums.length; k++) {
            List<Integer> l = new ArrayList();
            backtracking(nums, k, ll, l);
        }
        
        return ll;
    }
}
```

---
#### 79. Word Search 

**Description**   
>Given a 2D board and a word, find if the word exists in the grid.  
>The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell >may not be used more than once.    
>For example,  
>Given board =  
```
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
```
>word = "ABCCED", -> returns true,  
>word = "SEE", -> returns true,  
>word = "ABCB", -> returns false.     

**Solution**  
**思路**  
查找到一个解就返回，返回boolean值的graph DFS。 
返回boolean的dfs，因为go to next choice 是先后顺序的，因此one of next choice返回解（return true），current choice返回true，而避免继续下一个choice。
**以下代码比较冗长**   
```java
public class Solution {
    public boolean dfs(char[][]board, boolean[][] map, String word, int i, int j, int length) {
        
        if (length == word.length()) {
            return true;
        }
        if (board[i][j] == word.charAt(length)) {
            map[i][j] = true;
            if (length + 1 == word.length()) {
                map[i][j] = false;
                return true;
            }
            if (i - 1 >= 0 && !map[i - 1][j]) {
                if (dfs(board, map, word, i - 1, j, length + 1)) {
                    map[i][j] = false;
                    return true;
                }
            } 
            if (i + 1 < map.length && !map[i + 1][j]){
                if (dfs(board, map, word, i + 1, j, length + 1)) {
                    map[i][j] = false;
                    return true;
                }
            }
            if (j - 1 >= 0 && !map[i][j - 1]) {
                if (dfs(board, map, word, i, j - 1, length + 1)) {
                    map[i][j] = false;
                    return true;
                }
            }
            if (j + 1 < map[0].length && !map[i][j + 1]) {
                if (dfs(board, map, word, i, j + 1, length + 1)) {
                    map[i][j] = false;
                    return true;
                }
            }
            map[i][j] = false;
        }
        return false;
    }
    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        boolean[][] map = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == word.charAt(0) && dfs(board, map, word, i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

**修改后**   

```java
public class Solution {
    public boolean dfs(char[][]board, boolean[][] map, String word, int i, int j, int pos) {
        // leaf node, out of boundary or find goal
        if (pos == word.length()) {
            return true;
        }
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || map[i][j] || board[i][j] != word.charAt(pos)) {
            return false;
        }
        map[i][j] = true;
        if (dfs(board, map, word, i - 1, j, pos + 1) ||
            dfs(board, map, word, i + 1, j, pos + 1) || 
            dfs(board, map, word, i, j - 1, pos + 1) || 
            dfs(board, map, word, i, j + 1, pos + 1)) {
                return true;
        }
        map[i][j] = false;
        return false;
     }
    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        boolean[][] map = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == word.charAt(0) && dfs(board, map, word, i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
---
#### 90. Subsets II 

__Description__   
>Given a collection of integers that might contain duplicates, nums, return all possible subsets.
>
>Note: The solution set must not contain duplicate subsets.
>
>For example,
>If nums = [1,2,2], a solution is:
```
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```


__Solution__    
DFS. all nodes as leaf node. path作为摆渡船。 注意去重`i == current || nums[i] != nums[i-1]`。   

**代码**  
```java
public class Solution {
    public void dfs(int[] nums, 
                             int current, 
                             List<List<Integer>> result, 
                             List<Integer> path) {
        // all node is leaf node, but should not be stoped
        result.add(new ArrayList<Integer>(path));
        // part II, go to next choice 
        for (int i = current; i < nums.length; i++) { // ignor duplicates 
            if (i == current || nums[i] != nums[i-1]) { 
                path.add(nums[i]);
                dfs(nums, i + 1, result, path);
                path.remove(path.size() - 1);
            }
        }
    }
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList();
        List<Integer> path = new ArrayList<Integer>();
        dfs(nums, 0, result, path);
        return result;
    }
}
```
**第一次代码，太冗长**  
```java
public class Solution {
    public void backtracking(int[] nums, 
                             int cursor, 
                             List<List<Integer>> ll, 
                             List<Integer> l) {
        ll.add(l);
        for (int i = cursor; i < nums.length; i++) { // cursor index to first element could be added
            if (i == cursor || nums[i] != nums[i-1]) {
                List<Integer> newList = new ArrayList();
                newList.addAll(l);
                newList.add(nums[i]);
                backtracking(nums, i + 1, ll, newList);
            }
        }
    }
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ll = new ArrayList();
        List<Integer> l = new ArrayList();
        backtracking(nums, 0, ll, l);
        return ll;
    }
}
```
---
#### 200. Number of Islands

__Description__   
>Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

>Example 1:
```
11110
11010
11000
00000
```
>Answer: 1

>Example 2:
```
11000
11000
00100
00011
```
>Answer: 3 


__Solution__  
思路：DFS。从一个节点开始DFS，并将访问过的点标记。记录总的label数量。
```java
public class Solution {
    public void dfs(char[][] grid, boolean[][] visited, int i, int j) {
        // i-1, j-1, i+1, j+1
        if (i < 0 || 
            j < 0 || 
            i >= grid.length || 
            j >= grid[0].length ||
            visited[i][j] ||
            grid[i][j] == '0') {// out of boundary or visited
            return;
        }
        // mark current
        visited[i][j] = true;
        // go to next choices
        dfs(grid, visited, i - 1, j);
        dfs(grid, visited, i + 1, j);
        dfs(grid, visited, i, j - 1);
        dfs(grid, visited, i, j + 1);
        // do not back track
    }
    public int numIslands(char[][] grid) {
        if (grid == null ||
            grid.length == 0 ||
            grid[0] == null || 
            grid[0].length == 0) {
                return 0;
        }
        
        int labels = 0;
        int m = grid.length;
        int n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1' && !visited[i][j]) {
                    labels++;
                    dfs(grid, visited, i, j);
                }
            }
        }
        return labels;
    }
}
```
**if we could modify the grid**  
```java
public class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0] == null || grid[0].length == 0) {
            return 0;
        }
        int count = 0;
        int m = grid.length;
        int n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    dfs(grid, i, j);
                }
            }
        }
        return count;
    }
    public void dfs(char[][] grid, int i, int j) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != '1') {
            return;
        }
        grid[i][j] = '0';
        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j - 1);
        dfs(grid, i, j + 1);
    }
}

```
思路2：不相交集合
```java

```
---
#### 216. Combination Sum III 

**Description**   
>Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.
Example 1:  
Input: k = 3, n = 7  
Output:  
`[[1,2,4]] ` 
Example 2:  
Input: k = 3, n = 9  
Output:  
`[[1,2,6], [1,3,5], [2,3,4]]`  


**Solution**  
**思路**  
DFS + sort(维护current指针去重)。

```java
public class Solution {
    public void dfs(int k, int n, int current, List<List<Integer>> result, List<Integer> path) {
        // leaf node, stop digging
        if (k <= 0 || n <= 0) {
            // goal node, find solution
            if (k == 0 && n == 0) {
                result.add(new ArrayList<Integer>(path));
            }
            return;
        }
        // current node (k -= 1, n -= i) and go to next choices
        for (int i = current; i<=9; i++) {
            if (i * k <= n) {
                path.add(i);
                dfs(k - 1, n - i, i + 1, result, path);
                path.remove(path.size() - 1);
            }
           
        }
    }
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        List<Integer> path = new ArrayList<Integer>();
        dfs(k, n, 1, result, path);
        return result;
    }
}
```
---
#### 241. Different Ways to Add Parentheses 

**Description**   
>Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are +, - and *.
>
>Example 1
>Input: "2-1-1".
```
((2-1)-1) = 0
(2-(1-1)) = 2
```
>Output: `[0, 2]`
>
>Example 2
>Input: "2*3-4*5"
```
(2*(3-(4*5))) = -34
((2*3)-(4*5)) = -14
((2*(3-4))*5) = -10
(2*((3-4)*5)) = -10
(((2*3)-4)*5) = 10
```
Output: `[-34, -14, -10, -10, 10]`

**Solution**  
**思路**  

**DFS | Divide and Conquer**  
```java
public class Solution {
    public List<Integer> divideConquer(String input) {
        List<Integer> result = new ArrayList<Integer>();
        for (int i = 0; i < input.length(); i++) {
            char c = input.charAt(i);
            if (!Character.isDigit(c)) { //there is no operator in input string
                List<Integer> left = divideConquer(input.substring(0, i));
                List<Integer> right = divideConquer(input.substring(i + 1, input.length()));
                result.addAll(merge(left, right, c));
            }
        }
        if (result.size() == 0) {
            result.add(Integer.parseInt(input));
        }
        return result;
    }
    public List<Integer> merge(List<Integer> left, List<Integer> right, char c) {
        List<Integer> result = new ArrayList<Integer>();
        for (int i = 0; i < left.size(); i++) {
            for (int j = 0; j < right.size(); j++) {
                int x = left.get(i);
                int y = right.get(j);
                if (c == '*') {
                    result.add(x * y);
                } else if (c == '-') {
                    result.add(x - y);
                } else if (c == '+') {
                    result.add(x + y);
                }
            }
        }
        return result;
    }
    public List<Integer> diffWaysToCompute(String input) {
        return divideConquer(input);
    }
}
```
**优化**   
**记忆化搜索，就是讲子问题计算的结果保存（如，保存到hash table），计算子问题之前先查找已经被计算过。**  
**[记忆化搜索](http://www.cnblogs.com/fu11211129/p/4276213.html)的文章**
```java
public class Solution {
    public List<Integer> divideConquer(String input, HashMap<String, List<Integer>> dp) {
        if (dp.containsKey(input)) {
            return dp.get(input);
        }
        List<Integer> result = new ArrayList<Integer>();
        for (int i = 0; i < input.length(); i++) {
            char c = input.charAt(i);
            if (!Character.isDigit(c)) { //there is no operator in input string
                List<Integer> left = divideConquer(input.substring(0, i));
                List<Integer> right = divideConquer(input.substring(i + 1, input.length()));
                result.addAll(merge(left, right, c, input));
            }
        }
        if (result.size() == 0) {
            result.add(Integer.parseInt(input));
        }
        dp.put(input, result);
        return result;
    }
    public List<Integer> merge(List<Integer> left, List<Integer> right, char c) {
        List<Integer> result = new ArrayList<Integer>();
        for (int i = 0; i < left.size(); i++) {
            for (int j = 0; j < right.size(); j++) {
                int x = left.get(i);
                int y = right.get(j);
                if (c == '*') {
                    result.add(x * y);
                } else if (c == '-') {
                    result.add(x - y);
                } else if (c == '+') {
                    result.add(x + y);
                }
            }
        }
        return result;
    }
    public List<Integer> diffWaysToCompute(String input) {
        HashMap<String, List<Integer>> dp = new HashMap<String, List<Integer>>();
        return divideConquer(input, dp);
    }
}
```
---
#### 282. Expression Add Operators 

**Description**   
>Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

>Examples: 
```
"123", 6 -> ["1+2+3", "1*2*3"] 
"232", 8 -> ["2*3+2", "2+3*2"]
"105", 5 -> ["1*0+5","10-5"]
"00", 0 -> ["0+0", "0-0", "0*0"]
"3456237490", 9191 -> []
```

**[题目链接](https://leetcode.com/problems/expression-add-operators/)**  

**Solution**  
**思路**    
subsets类问题。用dfs将num划分，在两个数之前填入运算符。  
一、划分。  
有多少种划分呢？ 2 ^ (n - 1)  - 1  
```
1 2 3 4
2 ^ 3 - 1
```
二、运算  
借鉴eval用stack计算值。     
对于一个comming number num，看栈顶是：  
1. `null`，直接入栈  
2. `"*"`, `stack.pop()*num`入栈  
3. `"+"`, 将栈内所有元素pop求和，然后`num`入栈  
4. `"-"`,将栈内所有元素pop求和，然后`-num`入栈    

**其实stack的size最大是2.**在加入commingnumber之前，先将已遍历部分计算然后入栈。  
可用sum表示已经计算的值，prev表示上一个数(还没有加入到sum中去)，参照eval从左到右扫描即可。    
```java
public class Solution {
    public List<String> addOperators(String num, int target) {
        List<String> result = new ArrayList();
        if (num == null || num.length() == 0) {
            return result;
        }
        String path = new String("");
        dfs(num, target, 0, 0, 0, result, path);
        return result;
    }
    public void dfs(String num, int target, int pos, long sum, long prev, List<String> result, String path) {
        // leaf node, end of the string
        if (pos == num.length()) {
            if (prev + sum == target) {
                result.add(path);
            }
            return;
        }
        // see what operater btwn prev number and current number
        for (int i = pos; i < num.length(); i++) {
            String s = num.substring(pos, i + 1);
            long x = Long.parseLong(s);
            if (pos == 0) {
                dfs(num, target, i + 1, 0, x, result, path + s);
            } else {
                // +
                dfs(num, target, i + 1, sum + prev, x, result, path + "+" + s);
                // -
                dfs(num, target, i + 1, sum + prev, -x, result, path + "-" + s);
                // *
                dfs(num, target, i + 1, sum, prev * x, result, path + "*" + s);
            }
            // this expression used to prevent to calculate 00....
            //  if the first char is '0', end loop
            if (num.charAt(pos) == '0') {
                break;
            }
        }
    }
}
```
* * * 
