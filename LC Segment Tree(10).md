#### 201. Segment Tree Build

**Description**   
>The structure of Segment Tree is a binary tree which each node has two attributes start and end denote an segment / interval.

>start and end are both integers, they should be assigned in following rules:

>The root's start and end is given by build method.
The left child of node A has start=A.left, end=(A.left + A.right) / 2.
>The right child of node A has start=(A.left + A.right) / 2 + 1, end=A.right.
if start equals to end, there will be no children for this node.
>Implement a build method with two parameters start and end, so that we can create a corresponding segment tree with every node has the correct start and end value, return the root of this segment tree.

**[题目链接](https://www.lintcode.com/en/problem/segment-tree-build)**  
**Solution**  
**思路**  


**代码**   
```java
/**
 * Definition of SegmentTreeNode:
 * public class SegmentTreeNode {
 *     public int start, end;
 *     public SegmentTreeNode left, right;
 *     public SegmentTreeNode(int start, int end) {
 *         this.start = start, this.end = end;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     *@param start, end: Denote an segment / interval
     *@return: The root of Segment Tree
     */
    public SegmentTreeNode build(int start, int end) {
        // write your code here
        if (start > end) {
            return null;
        }
        SegmentTreeNode node = new SegmentTreeNode(start, end);
        if (start == end) {
            return node;
        }
        SegmentTreeNode left = build(start, (start + end) / 2);
        SegmentTreeNode right = build((start + end) / 2 + 1, end);
        node.left = left;
        node.right = right;
        return node;
    }
}
```
* * *
#### 202. Segment Tree Query

**Description**   
>For an integer array (index from 0 to n-1, where n is the size of this array), in the corresponding SegmentTree, each node stores an extra attribute max to denote the maximum number in the interval of the array (index from start to end).
>
>Design a query method with three parameters root, start and end, find the maximum number in the interval [start, end] by the given root of segment tree.
>Example
>For array [1, 4, 2, 3], the corresponding Segment Tree is:

```

                  [0, 3, max=4]
                 /             \
          [0,1,max=4]        [2,3,max=3]
          /         \        /         \
   [0,0,max=1] [1,1,max=4] [2,2,max=2], [3,3,max=3]
```
query(root, 1, 1), return 4

query(root, 1, 2), return 4

query(root, 2, 3), return 3

query(root, 0, 2), return 4

**[题目链接](https://www.lintcode.com/en/problem/segment-tree-query/#)**  
**Solution**  
**思路**  


**代码**   
```java
/**
 * Definition of SegmentTreeNode:
 * public class SegmentTreeNode {
 *     public int start, end, max;
 *     public SegmentTreeNode left, right;
 *     public SegmentTreeNode(int start, int end, int max) {
 *         this.start = start;
 *         this.end = end;
 *         this.max = max
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     *@param root, start, end: The root of segment tree and 
     *                         an segment / interval
     *@return: The maximum number in the interval [start, end]
     */
    public int query(SegmentTreeNode root, int start, int end) {
        // write your code here
        if (root == null || start > root.end || end < root.start || start > end) {
            return Integer.MIN_VALUE;
        }
        if (start == root.start && end == root.end || root.start == root.end) {
            return root.max;
        }
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            int left = query(root.left, start, mid);
            int right = query(root.right, mid + 1, end);
            return Math.max(left, right);
        }
        if (end <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}
```
* * *
####  203. Segment Tree Modify

**Description**   
>For a Maximum Segment Tree, which each node has an extra value max to store the maximum value in this node's interval.
>
>Implement a modify function with three parameter root, index and value to change the node's value with [start, end] = [index, index] to the new given value. Make sure after this change, every node in segment tree still has the max attribute with the correct value.
>Example
For segment tree:

```
                      [1, 4, max=3]
                    /                \
        [1, 2, max=2]                [3, 4, max=3]
       /              \             /             \
[1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=3]
if call modify(root, 2, 4), we can get:

                      [1, 4, max=4]
                    /                \
        [1, 2, max=4]                [3, 4, max=3]
       /              \             /             \
[1, 1, max=2], [2, 2, max=4], [3, 3, max=0], [4, 4, max=3]
or call modify(root, 4, 0), we can get:

                      [1, 4, max=2]
                    /                \
        [1, 2, max=2]                [3, 4, max=0]
       /              \             /             \
[1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=0]
```

**[题目链接]()**  
**Solution**  
**思路**  


**代码**   
```java
/**
 * Definition of SegmentTreeNode:
 * public class SegmentTreeNode {
 *     public int start, end, max;
 *     public SegmentTreeNode left, right;
 *     public SegmentTreeNode(int start, int end, int max) {
 *         this.start = start;
 *         this.end = end;
 *         this.max = max
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     *@param root, index, value: The root of segment tree and 
     *@ change the node's value with [index, index] to the new given value
     *@return: void
     */
    public void modify(SegmentTreeNode root, int index, int value) {
        // write your code here
        if (root.start == root.end) {
            root.max = value;
            return;
        }
        int mid = (root.start + root.end) / 2;
        if (index <= mid) {
            modify(root.left, index, value);
        } else {
            modify(root.right, index, value);
        }
        root.max = Math.max(root.left.max, root.right.max);
    }
}
```
* * *
#### 205. Interval Minimum Number

**Description**   
>Given an integer array (index from 0 to n-1, where n is the size of this array), and an query list. Each query has two integers [start, end]. For each query, calculate the minimum number between index start and end in the given array, return the result list.
>Example
For array [1,2,7,8,5], and queries [(1,2),(0,4),(2,4)], return [2,1,5]

**[题目链接](https://www.lintcode.com/en/problem/interval-minimum-number/#)**  
**Solution**  
**思路**  
Segment Tree. Build and query.  

**代码**   
```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */
class SegmentNode {
     int start;
     int end;
     int min;
     SegmentNode left;
     SegmentNode right;
     public SegmentNode(int start, int end, int min) {
         this.start = start;
         this.end = end;
         this.left = null;
         this.right = null;
         this.min = min;
     }
}
public class Solution {
    /**
     *@param A, queries: Given an integer array and an query list
     *@return: The result list
     */
    
    public ArrayList<Integer> intervalMinNumber(int[] A, 
                                                ArrayList<Interval> queries) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (A == null || A.length == 0 || queries == null || queries.size() == 0) {
            return result;
        }
        SegmentNode root = build(A, 0, A.length - 1);
        for (Interval in : queries) {
            result.add(query(root, in.start, in.end));
        }
        return result;
    }
    public SegmentNode build(int[] A, int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, A[start]);
        }
        SegmentNode root = new SegmentNode(start, end, Integer.MAX_VALUE);
        int mid = (start + end) / 2;
        SegmentNode left = build(A, start, mid);
        SegmentNode right = build(A, mid + 1, end);
        root.left = left;
        root.right = right;
        root.min = Math.min(left.min, right.min);
        //System.out.println(start + " " + end + " " + root.min);
        return root;
    }
    public int query(SegmentNode root, int start, int end) {
        if (start == root.start && end == root.end || root.start == root.end) {
            return root.min;
        }
        
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            int left = query(root.left, start, mid);
            int right = query(root.right, mid + 1, end);
            return Math.min(left, right);
        }
        if (end <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}
```
* * *
#### 206. Interval Sum

**Description**   
>Given an integer array (index from 0 to n-1, where n is the size of this array), and an query list. Each query has two integers [start, end]. For each query, calculate the sum number between index start and end in the given array, return the result list.
>Example
> For array [1,2,7,8,5], and queries [(0,4),(1,2),(2,4)], return [23,9,20]
**[题目链接](https://www.lintcode.com/en/problem/interval-sum/#)**  
**Solution**  
**思路**  
Segment Tree. 

**代码**   
```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */
class SegmentNode {
     int start;
     int end;
     long sum;
     SegmentNode left;
     SegmentNode right;
     public SegmentNode(int start, int end, long sum) {
         this.start = start;
         this.end = end;
         this.left = null;
         this.right = null;
         this.sum = sum;
     }
}
public class Solution {
    /**
     *@param A, queries: Given an integer array and an query list
     *@return: The result list
     */
    public ArrayList<Long> intervalSum(int[] A, 
                                       ArrayList<Interval> queries) {
        // write your code here
        ArrayList<Long> result = new ArrayList<Long>();
        if (A == null || A.length == 0 || queries == null || queries.size() == 0) {
            return result;
        }
        SegmentNode root = build(A, 0, A.length - 1);
        for (Interval in : queries) {
            result.add(query(root, in.start, in.end));
        }
        return result;
    }
    public SegmentNode build(int[] A, int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, A[start]);
        }
        SegmentNode root = new SegmentNode(start, end, 0);
        int mid = (start + end) / 2;
        SegmentNode left = build(A, start, mid);
        SegmentNode right = build(A, mid + 1, end);
        root.left = left;
        root.right = right;
        root.sum = left.sum + right.sum;
        //System.out.println(start + " " + end + " " + root.min);
        return root;
    }
    public long query(SegmentNode root, int start, int end) {
        if (start == root.start && end == root.end || root.start == root.end) {
            return root.sum;
        }
        
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            long left = query(root.left, start, mid);
            long right = query(root.right, mid + 1, end);
            return left + right;
        }
        if (end <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}

```
* * *
####  207. Interval Sum II

**Description**   
>Given an integer array in the construct method, implement two methods query(start, end) and modify(index, value):
>
>For query(start, end), return the sum from index start to index end in the given array.
>For modify(index, value), modify the number in the given index to value
>Example
>Given array A = [1,2,7,8,5].
>
>query(0, 2), return 10.
>modify(0, 4), change A[0] from 1 to 4.
>query(0, 1), return 6.
>modify(2, 1), change A[2] from 7 to 1.
>query(2, 4), return 14.

**[题目链接](https://www.lintcode.com/en/problem/interval-sum-ii/#)**  
**Solution**  
**思路**  
Segment Tree

**代码**   
```java
class SegmentNode {
    int start;
    int end;
    long sum;
    SegmentNode left;
    SegmentNode right;
    public SegmentNode(int start, int end, long sum) {
        this.start = start;
        this.end = end;
        this.sum = sum;
        this.left = null;
        this.right = null;
    }
}
public class Solution {
    /* you may need to use some attributes here */
    SegmentNode segmentTree;
    
    /**
     * @param A: An integer array
     */
    public Solution(int[] A) {
        // write your code here
        this.segmentTree = build(A, 0, A.length - 1);
    }
    
    /**
     * @param start, end: Indices
     * @return: The sum from start to end
     */
    public long query(int start, int end) {
        // write your code here
        if (segmentTree == null || start > end || start > segmentTree.end || end < segmentTree.start) {
            return Long.MIN_VALUE;
        }
        return query(segmentTree, start, end);
    }
    public long query(SegmentNode root, int start, int end) {
        if (start  == root.start && end == root.end || root.start == root.end) {
            return root.sum;
        }
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            long left = query(root.left, start, mid);
            long right = query(root.right, mid + 1, end);
            return left + right;
        }
        if (start <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
    
    /**
     * @param index, value: modify A[index] to value.
     */
    public void modify(int index, int value) {
        // write your code here
        if (segmentTree == null || index > segmentTree.end || index < segmentTree.start) {
            return;
        }
        modify(segmentTree, index, value);
        
    }
    public void modify(SegmentNode root, int index, int value) {
        if (index > root.end || index < root.start) {
            return;
        }
        if (root.start == root.end) {
            root.sum = value;
            return;
        }
        int mid = (root.start + root.end) / 2;
        if (index <= mid) {
            modify(root.left, index, value);
        } else {
            modify(root.right, index, value);
        }
        root.sum = root.left.sum + root.right.sum;
    }
    public SegmentNode build(int[] A, int start, int end) {
        if (A == null || A.length == 0 || start > end) {
            return null;
        }
        return buildHelper(A, start, end);
    }
    public SegmentNode buildHelper(int[] A, int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, A[start]);
        }
        SegmentNode root = new SegmentNode(start, end, 0);
        int mid = (start + end) / 2;
        SegmentNode left = buildHelper(A, start, mid);
        SegmentNode right = buildHelper(A, mid + 1, end);
        root.left = left;
        root.right = right;
        root.sum = left.sum + right.sum;
        return root;
    }
}
```
* * *
####  247. Segment Tree Query II

**Description**   
>For an array, we can build a SegmentTree for it, each node stores an extra attribute count to denote the number of elements in the the array which value is between interval start and end. (The array may not fully filled by elements)
>
>Design a query method with three parameters root, start and end, find the number of elements in the in array's interval [start, end] by the given root of value SegmentTree.
>Example
>For array [0, 2, 3], the corresponding value Segment Tree is:

```
                     [0, 3, count=3]
                     /             \
          [0,1,count=1]             [2,3,count=2]
          /         \               /            \
   [0,0,count=1] [1,1,count=0] [2,2,count=1], [3,3,count=1]
```
>query(1, 1), return 0
>
>query(1, 2), return 1
>
>query(2, 3), return 2
>
>query(0, 2), return 2

**[题目链接](https://www.lintcode.com/en/problem/segment-tree-query-ii/#)**  
**Solution**  
**思路**  


**代码**   
```java
/**
 * Definition of SegmentTreeNode:
 * public class SegmentTreeNode {
 *     public int start, end, count;
 *     public SegmentTreeNode left, right;
 *     public SegmentTreeNode(int start, int end, int count) {
 *         this.start = start;
 *         this.end = end;
 *         this.count = count;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     *@param root, start, end: The root of segment tree and 
     *                         an segment / interval
     *@return: The count number in the interval [start, end]
     */
    public int query(SegmentTreeNode root, int start, int end) {
        // write your code here
        if (root == null || start > root.end || end < root.start || start > end) {
            return 0;
        }
        if (start == root.start && end == root.end  || root.start  == root.end) {
            return root.count;
        }
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            int left = query(root.left, start, mid);
            int right = query(root.right, mid + 1, end);
            return left + right;
        }
        if (end <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}
```
* * *
#### 248. Count of Smaller Number

**Description**   
>Give you an integer array (index from 0 to n-1, where n is the size of this array, value from 0 to 10000) and an query list. For each query, give you an integer, return the number of element in the array that are smaller than the given integer.
>Example
>For array [1,2,7,8,5], and queries [1,8,5], return [0,4,2]

**[题目链接](https://www.lintcode.com/en/problem/count-of-smaller-number/#)**  
**Solution**  
**思路**  
Segment Tree.

**代码**   
```java
class SegmentNode {
    int start;
    int end;
    int value;
    SegmentNode left;
    SegmentNode right;
    public SegmentNode(int start, int end, int value) {
        this.start = start;
        this.end = end;
        this.value = value;
    }
}
public class Solution {
   /**
     * @param A: An integer array
     * @return: The number of element in the array that 
     *          are smaller that the given integer
     */
    public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (queries == null || queries.length == 0) {
            return result;
        }
        SegmentNode root = build(0, 10000);
        for (int v : A) {
            modify(root, v);
        }

        for (int q : queries) {
            result.add(query(root, 0, q - 1));
        }
        return result;
    }
    public SegmentNode build(int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, 0);
        }
        int mid = (start + end) / 2;
        SegmentNode root = new SegmentNode(start, end, 0);
        SegmentNode left = build(start, mid);
        SegmentNode right = build(mid + 1, end);
        root.left = left;
        root.right = right;
        return root;
    }
    public void modify(SegmentNode root, int value) {
        if (root.start == root.end) {
            if (root.start == value) {
                root.value += 1;
            }
            return;
        }
        int mid = (root.start + root.end) / 2;
        if (value <= mid) {
            modify(root.left, value);
        } else {
            modify(root.right, value);
        }
        root.value = root.left.value + root.right.value;
    }
    public int query(SegmentNode root, int start, int end) {
        if (start == root.start && end == root.end || root.start == root.end) {
            return root.value;
        }
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            int left = query(root.left, start, mid);
            int right = query(root.right, mid + 1, end);
            return left + right;
        }
        if (start <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}

```
* * *
#### 249. Count of Smaller Number before itself

**Description**   
>Give you an integer array (index from 0 to n-1, where n is the size of this array, value from 0 to 10000) . For each element Ai in the array, count the number of element before this element Ai is smaller than it and return count number array.
>Example
>For array [1,2,7,8,5], return [0,1,2,3,2]

**[题目链接](https://www.lintcode.com/en/problem/count-of-smaller-number-before-itself/#)**  
**Solution**  
**思路**  
Segment Tree. modify the tree and at the same time, query the interval[0, v - 1].

**代码**   
```java
class SegmentNode {
    int start;
    int end;
    int value;
    SegmentNode left;
    SegmentNode right;
    public SegmentNode(int start, int end, int value) {
        this.start = start;
        this.end = end;
        this.value = value;
    }
}
public class Solution {
   /**
     * @param A: An integer array
     * @return: The number of element in the array that 
     *          are smaller that the given integer
     */
    public ArrayList<Integer> countOfSmallerNumberII(int[] A) {
        // write your code here
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (A == null || A.length == 0) {
            return result;
        }
        SegmentNode root = build(0, 10000);
        for (int v : A) {
            int cnt = 0;
            if (v > 0) {
                cnt = query(root, 0, v - 1);
            }
            modify(root, v);
            result.add(cnt);
        }
        return result;
    }
    public SegmentNode build(int start, int end) {
        if (start == end) {
            return new SegmentNode(start, end, 0);
        }
        int mid = (start + end) / 2;
        SegmentNode root = new SegmentNode(start, end, 0);
        SegmentNode left = build(start, mid);
        SegmentNode right = build(mid + 1, end);
        root.left = left;
        root.right = right;
        return root;
    }
    public void modify(SegmentNode root, int value) {
        if (root.start == root.end) {
            if (root.start == value) {
                root.value += 1;
            }
            return;
        }
        int mid = (root.start + root.end) / 2;
        if (value <= mid) {
            modify(root.left, value);
        } else {
            modify(root.right, value);
        }
        root.value = root.left.value + root.right.value;
    }
    public int query(SegmentNode root, int start, int end) {
        if (start == root.start && end == root.end || root.start == root.end) {
            return root.value;
        }
        int mid = (root.start + root.end) / 2;
        if (start <= mid && end >= mid + 1) {
            int left = query(root.left, start, mid);
            int right = query(root.right, mid + 1, end);
            return left + right;
        }
        if (start <= mid) {
            return query(root.left, start, end);
        }
        return query(root.right, start, end);
    }
}

```
* * *
####  439. Segment Tree Build II

**Description**   
>The structure of Segment Tree is a binary tree which each node has two attributes start and end denote an segment / interval.
>
>start and end are both integers, they should be assigned in following rules:

>The root's start and end is given by build method.
The left child of node A has start=A.left, end=(A.left + A.right) / 2.
>The right child of node A has start=(A.left + A.right) / 2 + 1, end=A.right.
if start equals to end, there will be no children for this node.
>Implement a build method with a given array, so that we can create a corresponding segment tree with every node value represent the corresponding interval max value in the array, return the root of this segment tree.

**[题目链接](https://www.lintcode.com/en/problem/segment-tree-build-ii/)**  
**Solution**  
**思路**  

**代码**   
```java
/**
 * Definition of SegmentTreeNode:
 * public class SegmentTreeNode {
 *     public int start, end, max;
 *     public SegmentTreeNode left, right;
 *     public SegmentTreeNode(int start, int end, int max) {
 *         this.start = start;
 *         this.end = end;
 *         this.max = max
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     *@param A: a list of integer
     *@return: The root of Segment Tree
     */
    public SegmentTreeNode build(int[] A) {
        // write your code here
        // corner case
        if (A == null || A.length == 0) {
            return null;
        }
        return build(A, 0, A.length - 1);
    }
    public SegmentTreeNode build(int[] A, int start, int end) {
        if (start > end) {
            return null;
        }
        if (start == end) {
            return new SegmentTreeNode(start, end, A[start]);
        }
        SegmentTreeNode node = new SegmentTreeNode(start, end, Integer.MIN_VALUE);
        SegmentTreeNode left = build(A, start, (start + end) / 2);
        SegmentTreeNode right = build(A, (start + end) / 2 + 1, end);
        node.left = left;
        node.right = right;
        node.max = Math.max(left.max, right.max);
        return node;
    }
}
```
* * *
