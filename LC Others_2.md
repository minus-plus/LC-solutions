#### 84. Largest Rectangle in Histogram  

**Description**   
>Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.
>![histo](http://www.leetcode.com/wp-content/uploads/2012/04/histogram.png)
>Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].
>
>The largest rectangle is shown in the shaded area, which has area = 10 unit.
>![histo](http://www.leetcode.com/wp-content/uploads/2012/04/histogram_area.png)
>For example,
>Given heights = [2,1,5,6,2,3],
>return 10.

**[题目链接](https://leetcode.com/problems/largest-rectangle-in-histogram/)**  
**Solution**  
**思路**  
1 暴力枚举i, j，计算每个区间，复杂度O(n^3)。  
2  用max-stack方式，要求栈内元素按升序排列，遇到值小的元素入栈前，要讲所有比comming element（index is i）大的元素pop出。每pop除一个元素，都要计算stack.peek元素到i - 1之间组成的矩形的大小。复杂度O(n)。每个元素入栈一次，出栈一次。
```
h = stack.pop();
w = i - 1 - stack.peek(); 
area = h * w; 
max = Math.max(max, area);
```

**代码**   
```java
public class Solution {
    public int largestRectangleArea(int[] heights) {
        if (heights == null || heights.length == 0) {
            return 0;
        }
        Stack<Integer> stack = new Stack<Integer>();
        int max = 0;
        for (int i = 0; i <= heights.length; i++) {
            int current = i == heights.length ? -1 : heights[i];
            while (!stack.empty() && current <= heights[stack.peek()]) {
                int h = heights[stack.pop()];
                int w = stack.empty() ? i : i - stack.peek() - 1;
                max = Math.max(max, h * w);
            }
            stack.push(i);
        }
        return max;
    }
}
```
* * *

#### 85. Maximal Rectangle  

**Description**   
>Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.
>
>For example, given the following matrix:
```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```
Return 6.

**[题目链接](https://leetcode.com/problems/maximal-rectangle/)**  
**Solution**  
**思路**  
最开始想采用Maximal Square的方法，dp，发现没有递归关系。放弃。  
类比84题，采用逐行扫描的方式，将每一行看做柱状图，计算每一行的最大矩形。求最大值即可。  
首先要统计每行的柱状图heights[]。用dp即可。  

**代码**   
```java

public class Solution {
    public int maximalRectangle(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0] == null || matrix[0].length == 0) {
            return 0;
        }
        int m = matrix.length;
        int n = matrix[0].length;
        int[][] height = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0) {
                    height[i][j] = matrix[i][j] == '1' ? 1 : 0;
                } else {
                    height[i][j] = matrix[i][j] == '1' ? height[i - 1][j] + 1 : 0;
                }
            }
        }
        int max = 0;
        for (int i = 0; i < m; i++) {
            Stack<Integer> stack = new Stack<Integer>();
            for (int j = 0; j <= n; j++) {
                int current = j == n ? -1 : height[i][j];
                while (!stack.empty() && current < height[i][stack.peek()]) {
                    int h = height[i][stack.pop()];
                    int w = stack.empty() ? j : j - stack.peek() - 1;
                    max = Math.max(max, h * w);
                }
                stack.push(j);
            }
        }
        return max;
    }
}
```
* * *

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
思路2：不相交集合
```java
public class Solution {
    private int[][] id;
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0] == null || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length;
        int n = grid[0].length;
        // init
        
        id = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                id[i][j] = i * n + j;
            }
        }
        // union
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    if (i > 0 && grid[i - 1][j] == '1') {
                        union(i, j, i - 1, j);
                    }
                    if (j > 0 && grid[i][j - 1] == '1') {
                        union(i, j, i, j - 1);
                    }
                }
            }
        }
        // find 
        Set<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1')
                set.add(find(i, j));
            }
        }
        return set.size();
    }
    public int find(int i, int j) {
        if (id[i][j] == i * id[0].length + j) {
            return id[i][j];
        }
        id[i][j] = find(id[i][j] / id[0].length, id[i][j] % id[0].length);
        return id[i][j];
    }
    public void union(int r0, int c0, int r1, int c1) {
        
        int f0 = find(r0, c0);
        int f1 = find(r1, c1);
        //System.out.println(f0 + " " + f1);
        id[f0 / id[0].length][f0 % id[0].length] = f1;
    }
}

```
* * *

