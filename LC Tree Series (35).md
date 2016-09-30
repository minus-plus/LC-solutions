94 Binary Tree Inorder Traversal  
95 Unique Binary Search Trees II  
96 Unique Binary Search Trees  
98 Validate Binary Search Tree  
99 Recover Binary Search Tree  
100 Same Tree  
101 Symmetric Tree  
102 Binary Tree Level Order Traversal  
103 Binary Tree Zigzag Level Order Traversal  
104 Maximum Depth of Binary Tree  
105 Construct Binary Tree from Preorder and Inorder Traversal  
106 Construct Binary Tree from Inorder and Postorder Traversal  
107 Binary Tree Level Order Traversal II  
108 Convert Sorted Array to Binary Search Tree  
109 Convert Sorted List to Binary Search Tree  
110 Balanced Binary Tree  
111 Minimum Depth of Binary Tree  
112 Path Sum  
113 Path Sum II  
114 Flatten Binary Tree to Linked List  
116 Populating Next Right Pointers in Each Node  
117 Populating Next Right Pointers in Each Node II  
124 Binary Tree Maximum Path Sum  
129 Sum Root to Leaf Numbers  
144 Binary Tree Preorder Traversal  
145 Binary Tree Postorder Traversal  
173 Binary Search Tree Iterator  
199 Binary Tree Right Side View  
222 Count Complete Tree Nodes  
226 Invert Binary Tree  
230 Kth Smallest Element in a BST  
235 Lowest Common Ancestor of a Binary Search Tree  
236 Lowest Common Ancestor of a Binary Tree  
257 Binary Tree Paths  
297 Serialize and Deserialize Binary Tree  

* * * 

#### 94. Binary Tree Inorder Traversal 

**Description**   
>Given a binary tree, return the inorder traversal of its nodes' values.
>
>For example:
>Given binary tree [1,null,2,3],
```
   1
    \
     2
    /
   3
```
return `[1,3,2]`.

**[题目链接](https://leetcode.com/problems/binary-tree-inorder-traversal/)**   
**Solution**  
**思路**  
非递归。顺序：  
1. left subtree go straight left untile null  
2. visit stack.peek(), current = stack.pop()
3. move to current.right as new root;

**代码非递归**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    // at node, if node.left != null, stack.push(node.left)
    // at node, if node != null, stack.push(node);
    // if node.left == null, current = stack.pop() 
    // if current.right == null, current = stack.pop()
    // if current.right != null, stack.push(current.right), current = current.right;
    public List<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> list = new ArrayList<Integer> ();
        Stack<TreeNode> stack = new Stack<TreeNode> ();
        if (root == null) {
            return list;
        }
        TreeNode current = root;
        while (current != null || !stack.empty()) {
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            current = stack.pop();
            list.add(current.val);
            current = current.right;
        }
        return list;
    }
}
```

**代码-recursive**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void inorderTraversalHelper (TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }
        if (root.left != null) {
            inorderTraversalHelper(root.left, list);
        }
        list.add(root.val);
        if (root.right != null) {
            inorderTraversalHelper(root.right, list);
        }
    }
    public List<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer> ();
        if (root == null) {
            return result;
        }
        inorderTraversalHelper(root, result);
        return result;
    }
}
```
* * *

#### 96. Unique Binary Search Trees 

**Description**   
>Given n, how many structurally unique BST's (binary search trees) that store values 1...n?
>
>For example,
>Given n = 3, there are a total of 5 unique BST's.
```java
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

**[题目链接](https://leetcode.com/problems/unique-binary-search-trees/)**    
**Solution**   
**思路**  
dp。  
二叉树的问题一般可用divide and conquer的思想来解决。以root为跟的二叉树的个数是以left child为跟的二叉树的个数 * 以right child为跟的二叉树的个数。翻译过来就是 。。。。  
`f(root) = f(left) * f(right)` 
设dp[i]为i个数组成的不同BST总数。  
如果左儿子个数为j， 右儿子显然为n - 1 - j个，那么总共显然有 `dp[j] * dp[i - 1 - j]`个。  
故枚举 j从0 ~ i - 1然后相加既可。边界i = 1。

**代码**   
```java
public class Solution {
    public int numTrees(int n) {
        if (n <= 0) {
            return 0;
        }
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= i - 1; j++) {
                dp[i] += dp[j] * dp[i - 1 - j];
            }
        }
        return dp[n];
    }
}
```
* * *
#### 95. Unique Binary Search Trees II 

**Description**   
>Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1...n.
>
>For example,
>Given n = 3, your program should return all 5 unique BST's shown below.
```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
	 ``` 
	 
	 
**[题目链接](https://leetcode.com/problems/unique-binary-search-trees-ii/)**  
**Solution**  
**思路**  
二叉树问题，一般可以用divide and conquer的思想来解决。   
divide: 在位置i，将序列分为两部分，0 ~ i - 1和i + 1 ~ n。 
conquer: i.left = left, i.right = right;   
要注意的部分：  
start > end时返回什么，为什么？  

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<TreeNode> divideConquer(int start, int end) {
        List<TreeNode> list = new ArrayList<TreeNode> ();
        if (start > end) {
            list.add(null);
            return list;
        }
        for (int i = start; i <= end; i++) {
            List<TreeNode> left = divideConquer(start, i - 1);
            List<TreeNode> right = divideConquer(i + 1, end);
            list.addAll(merge(left, right, i));
        }
        return list;
    }
    public List<TreeNode> merge(List<TreeNode> left, List<TreeNode> right, int i) {
        List<TreeNode> list = new ArrayList<TreeNode>();
        for (TreeNode l : left) {
            for (TreeNode r : right) {
                TreeNode node = new TreeNode(i);
                node.left = l;
                node.right = r;
                list.add(node);
            }
        }
        return list;
    }
    public List<TreeNode> generateTrees(int n) {
        if (n <= 0) {
            return new ArrayList<TreeNode>();
        }
        return divideConquer(1, n);
    }
}
```
**不单独写merge函数**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
 
public class Solution {
    /* 
    * divide and conquer, construct left subtree, construct right subtree, merge
    **/
    public ArrayList<TreeNode> generate(int start, int end) {
        ArrayList<TreeNode> list = new ArrayList<TreeNode>();
        if (start > end) {
            list.add(null);
            return list;
        }
        
        for (int i=start; i<=end; i++) {
            ArrayList<TreeNode> left = generate(start, i-1);
            ArrayList<TreeNode> right = generate(i+1, end);
            for (TreeNode l : left) {
                for (TreeNode r : right) {
                    TreeNode root = new TreeNode(i);
                    root.left = l;
                    root.right = r;
                    list.add(root);
                }
            }
        }
        return list;
    }
    public List<TreeNode> generateTrees(int n) {
        if (n <= 0) {
            return new ArrayList<TreeNode> ();
        }
        return generate(1, n);
    }
}
```
* * *

#### 98. Validate Binary Search Tree 

**Description**   
>Given a binary tree, determine if it is a valid binary search tree (BST).
>
>Assume a BST is defined as follows:
>
>The left subtree of a node contains only nodes with keys less than the node's key.
>The right subtree of a node contains only nodes with keys greater than the node's key.
>Both the left and right subtrees must also be binary search trees.
>Example 1:  
```
    2
   / \
  1   3
Binary tree [2,1,3], return true.
Example 2:
    1
   / \
  2   3
```
Binary tree [1,2,3], return false.   

**[题目链接](https://leetcode.com/problems/validate-binary-search-tree/)**  
**Solution**  
**思路**  
 1. Binary tree. use divide conquer.   
 2. inorder travers。current node has to larger than previous node.    

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
	public boolean divideConquer(TreeNode root, long min, long max) { // if use int, will not pass if one node has value of Integer.MIN_VALUE or Integer.MAX_LVAUE
        if (root == null) {
            return true;
        }
        if (root.val >= max || root.val <= min) {
            return false;
        }
        return divideConquer(root.left, min, root.val) && 
               divideConquer(root.right, root.val, max);
    }
    public boolean isValidBST(TreeNode root) {
       return divideConquer(root, (long)Integer.MIN_VALUE - 1, (long)Integer.MAX_VALUE + 1);
    }
}
```
**九章写法**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    class Result {
        boolean isValid;
        int max = Integer.MAX_VALUE;
        int min = Integer.MIN_VALUE;
        public Result(boolean isValid, int min, int max) {
            this.max = max;
            this.min = min;
            this.isValid = isValid;
        }
    }
    public Result divideConquer(TreeNode root) {
        if (root == null) {
            return new Result(true, Integer.MAX_VALUE, Integer.MIN_VALUE);
        }
        Result left = divideConquer(root.left);
        Result right = divideConquer(root.right);
        if (!left.isValid || !right.isValid ) {
            return new Result(false, 0, 0);
        }
        if (root.left != null && root.val <= left.max || root.right != null && root.val >= right.min) {
            return new Result(false, 0, 0);
        }
        return new Result(true, Math.min(left.min, root.val), Math.max(right.max, root.val));
    }
    public boolean isValidBST(TreeNode root) {
        return divideConquer(root).isValid;
    }
}
```
**inorder traverse**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        Stack<TreeNode> stack = new Stack<TreeNode>();
        TreeNode current = root;
        TreeNode prev = null;
        
        while (current != null || !stack.empty()) {
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            TreeNode node = stack.pop();
            if (prev != null && node.val <= prev.val) {
                return false;
            }
            prev = node;
            current = node.right;
        }
        return true;
    }
}
```
* * *
#### 99. Recover Binary Search Tree 

**Description**   
>Two elements of a binary search tree (BST) are swapped by mistake.
>
>Recover the tree without changing its structure.
>
>Note:
>A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?

**[题目链接](https://leetcode.com/problems/recover-binary-search-tree/)**  
**Solution**  
**思路**  
```
1 2 3 4 5 6 
1 2 6 4 5 3
```  
first element shuold be the `first prev` that prev.val >= node.val.  
second element should be the `last node` that prev.val >= node.val.   
用中序遍历可找到这两个node。   
**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void swap(TreeNode first, TreeNode second) {
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
    }
    public void recoverTree(TreeNode root) {
        TreeNode first = null;
        TreeNode second = null;
        
        TreeNode current = root;
        TreeNode prev = null;
        
        Stack<TreeNode> stack = new Stack<TreeNode>();
        
        while (current != null || !stack.empty()) {
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            TreeNode node = stack.pop();
            if (first == null && prev!= null && prev.val >= node.val) {
                first = prev;
            } else if (first != null && prev.val >= node.val) {
                second = node;
            }
            prev = node;
            current = node.right;
        }
        if (first != null && second != null) {
            swap(first, second);
        }
    }
}
```
* * *

#### 100. Same Tree 

**Description**   
>Given two binary trees, write a function to check if they are equal or not.
>Two binary trees are considered equal if they are structurally identical and the nodes have the same value.


**[题目链接](https://leetcode.com/problems/same-tree/)**  
**Solution**  
**思路**  
直接递归即可。  
**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        if (p != null && q != null) {
            return p.val == q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
        }
        return false;
    }
}
```
* * *
#### 101. Symmetric Tree 

**Description**   
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
But the following [1,2,2,null,3,null,3] is not:
```
    1
   / \
  2   2
   \   \
   3    3
```
**[题目链接](https://leetcode.com/problems/symmetric-tree/)**  
**Solution**  
**思路**  
思路1，直接递归。   
思路2，iterative。

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public boolean isSymmetricHelper(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        if (p != null && q != null) {
            return p.val == q.val && isSymmetricHelper(p.left, q.right) && isSymmetricHelper(p.right, q.left);
        }
        return false;
    }
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        return isSymmetricHelper(root.left, root.right);
    }
}
```
**non-recursive**  
left和right同时dfs。  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        Stack<TreeNode> stack1 = new Stack<TreeNode>();
        Stack<TreeNode> stack2 = new Stack<TreeNode>();
        stack1.push(root.left);
        stack2.push(root.right);
        
        while (!stack1.empty() && !stack2.empty()) {
            TreeNode node1 = stack1.pop();
            TreeNode node2 = stack2.pop();
            if (node1 != null && node2 != null) {
                if (node1.val != node2.val) {
                    return false;
                }
                stack1.push(node1.right);
                stack1.push(node1.left);
                stack2.push(node2.left);
                stack2.push(node2.right);
            } else if (node1 == null && node2 == null) {
               continue;
            } else {
                return false;
            }
        }
        return true;
    }
}
```

* * *
#### 102. Binary Tree Level Order Traversal 

**Description**   
>Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).
>
>For example:
>Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its level order traversal as:
```
[
  [3],
  [9,20],
  [15,7]
]
```

**[题目链接](https://leetcode.com/problems/binary-tree-level-order-traversal/)**  
**Solution**  
**思路**  
level order traversal。

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (root == null) {
            return result;
        }
        queue.offer(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            ArrayList<Integer> level = new ArrayList<Integer>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            result.add(level);
        }
        return result;
    }
}
```
* * *
#### 103. Binary Tree Zigzag Level Order Traversal 

**Description**   
>Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).
>
>For example:
>Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its zigzag level order traversal as:
```
[
  [3],
  [20,9],
  [15,7]
]
```

**[题目链接](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)**  
**Solution**  
**思路**  
102 level order traversal变形，隔一层要reverse level。 

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (root == null) {
            return result;
        }
        int flip = 0;
        queue.offer(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            ArrayList<Integer> level = new ArrayList<Integer>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            if (flip == 1) {
                Collections.reverse(level);
                flip = 0;
            } else {
                flip = 1;
            }
            result.add(level);
        }
        return result;
    }
}
```
* * *

#### 104. Maximum Depth of Binary Tree 

**Description**   
>Given a binary tree, find its maximum depth.
>
>The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.


**[题目链接](https://leetcode.com/problems/maximum-depth-of-binary-tree/)**  
**Solution**  
**思路**  
递归。  
BFS (level order traversal).   

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int maxDepth(TreeNode root) {
        // f(root) = max(f(left), f(right)) + 1; basis root == null, return 0, 
        if (root == null) {
            return 0;
        }
        return Math.max(maxDepth(root.left) , maxDepth(root.right)) + 1;
    }
}
```
**DFS**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int max = 0;
        Stack<TreeNode> stack = new Stack();
        Stack<Integer> depthStack = new Stack();
        stack.push(root);
        depthStack.push(1);
        while (!stack.empty()) {
            TreeNode node = stack.pop();
            int depth = depthStack.pop();
            max = Math.max(max, depth);
            if (node.right != null) {
                stack.push(node.right);
                depthStack.push(depth + 1);
            }
            if (node.left != null) {
                stack.push(node.left);
                depthStack.push(depth + 1);
            }
        }
        return max;
    }
}
```
**BFS** 
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int maxDepth(TreeNode root) {
        if (root==null) {
            return 0;
        }
        int depth = 0;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            depth++;
            for(int i = 0; i < size; i++){
                TreeNode node = queue.poll();
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return depth;
    }
}
```
* * *
#### 105. Construct Binary Tree from Preorder and Inorder Traversal

__Description__   
>Given preorder and inorder traversal of a tree, construct the binary tree.

>Note:
>You may assume that duplicates do not exist in the tree.

**[题目链接](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)**  
__Solution__      
**思路**   
preorder的第一个是root，inorder的mid是root，并将inorder分为左右子树。preorder也被分成左右两个子树，因此可以递归构建树。  
getIndex在inOrder中查找root的index，则左右子树的范围分别为：  
left: preStart+ 1, index - inStart + preStart, inStart, index - 1  
right: index - inStart + preStart + 1, preEnd, index + 1, inEnd
上述关系用index - 1 - inStart = preEndLeft - (preStart + 1) -> preEndLeft = index - inStart + preStart。 preStartRight = preEndLeft + 1。   

**代码**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int getIndex(int[] inOrder, int inStart, int inEnd, int val) {
        for (int i = inStart; i <= inEnd; i++) {
            if (inOrder[i] == val) {
                return i;
            }
        }
        return -1;
    }
    public TreeNode buildTreeHelper(int[] preOrder, int[] inOrder, int preStart, int preEnd, int inStart, int inEnd) {
        // basis or will never stop
        if (preStart > preEnd) {
            return null;
        }
        // preorder[preStart] is the root, and the root split inorder to left and right subtree
        TreeNode root = new TreeNode(preOrder[preStart]);
        int index = getIndex(inOrder, inStart, inEnd, preOrder[preStart]);
        TreeNode left = buildTreeHelper(preOrder, inOrder, preStart+ 1, index - inStart + preStart, inStart, index - 1) ;
        TreeNode right = buildTreeHelper(preOrder, inOrder, index - inStart + preStart + 1, preEnd, index + 1, inEnd);
        root.left = left;
        root.right = right;
        return root;
    }
    public TreeNode buildTree(int[] preOrder, int[] inOrder) {
        if (preorder == null || inorder == null || preorder.length == 0 || 
            inorder.length == 0 || preorder.length != inorder.length) {
            return null;
        }
        return buildTreeHelper(preOrder, inOrder, 0, preOrder.length - 1, 0, inOrder.length - 1);
    }
}
```
* * * 

#### 106. Construct Binary Tree from Inorder and Postorder Traversal

__Description__   
>Given inorder and postorder traversal of a tree, construct the binary tree.
>
>Note:
>You may assume that duplicates do not exist in the tree.

**[题目链接](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)**
__Solution__  
**思路** 
与105类似，只不过对postOrder来说，postOrder[postEnd]是root。  
**代码**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int getIndex(int[] inorder, int inStart, int inEnd, int val) {
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == val) {
                return i;
            }
        }
        return -1;
    }
    public TreeNode buildTreeHelper(int[] inorder, int[] postorder, int inStart, int inEnd, int postStart, int postEnd) {
        // basis, or will never stop
        if (postStart > postEnd) {
            return null;
        }
        // recursive relations:
        // postorder[postEnd] is the root, and split inorder and postorder into two parts
        // root.left = left; root.right = right; 
        TreeNode root = new TreeNode(postorder[postEnd]);
        int index = getIndex(inorder, inStart, inEnd, postorder[postEnd]);
        TreeNode left = buildTreeHelper(inorder, postorder, inStart, index - 1, postStart, postStart + index - inStart - 1);
        TreeNode right = buildTreeHelper(inorder, postorder, index + 1, inEnd, postStart + index - inStart, postEnd - 1);
        root.left = left;
        root.right = right;
        return root;
    }
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if (inorder == null || 
            postorder == null ||
            inorder.length == 0 ||
            postorder.length == 0) {
            return null;        
        }
        return buildTreeHelper(inorder, postorder, 0, inorder.length - 1, 0, postorder.length - 1);
    }
}
```
* * *

#### 107. Binary Tree Level Order Traversal II 

**Description**   
>Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).
>
>For example:
>Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its bottom-up level order traversal as:
```
[
  [15,7],
  [9,20],
  [3]
]
```

**[题目链接](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)**  
**Solution**  
**思路**  
类似于102，只不过要将结果逆序。你是有多水~~

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (root == null) {
            return result;
        }
        queue.offer(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            ArrayList<Integer> level = new ArrayList<Integer>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            result.add(level);
        }
        Collections.reverse(result);
        return result;
    }
}
```
* * *
#### 108. Convert Sorted Array to Binary Search Tree 

**Description**   
>Given an array where elements are sorted in ascending order, convert it to a height balanced BST.


**[题目链接](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)**    

**Solution**   
**思路**  
还是recursive relations。  
relations: 先找到root，将array分为左右两部分，root.left = left, root.right = right。   
basis: start > end  
水过~~  

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode generate(int[] nums, int start, int end) {
        if (start > end) {
            return null;
        }
        int mid = start + (end - start) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        TreeNode left = generate(nums, start, mid - 1);
        TreeNode right = generate(nums, mid + 1, end);
        root.left = left;
        root.right = right;
        return root;
    }
    public TreeNode sortedArrayToBST(int[] nums) {
        return generate(nums, 0, nums.length - 1);
    }
}
```
* * *
#### 109. Convert Sorted List to Binary Search Tree

__Description__   
>Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

**[题目链接](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree)**  

__Solution__   
**思路**  
依旧是二分。
divide and conquer.  
1. find middle of the list, as root. split to left and right.  
2. root.left = left, root.right = right;  
3. return root;  

思路1：将LinkedList转换为ArrayList，然后Divide and Conquer.   
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode dfs(List<Integer> list, int start, int end) {
        if (start > end) {
            return null;
        }
        // get mid
        int mid = start + (end - start) / 2;
        TreeNode root = new TreeNode(list.get(mid));
        TreeNode left = dfs(list, start, mid - 1);
        TreeNode right = dfs(list, mid + 1, end);
        root.left = left;
        root.right = right;
        return root;
    }
    
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) {
            return null;
        }
        List<Integer> list = new ArrayList();
        ListNode current = head;
        while (current != null) {
            list.add(current.val);
            current = current.next;
        }
        return dfs(list, 0, list.size() - 1);
        
    }
}
```
思路2：不将LinkedList转换为ArrayList。 超时。原因：right分支从head开始，多走冤枉路。
In this solution, start === 0, so parameter start could be removed. 
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode divideConquer(ListNode head, int start, int end) {
        if (start > end) {
            return null;
        }
        int mid = start + (end - start) / 2;
        int index = 0;
        ListNode current = head;
        while (index < mid) {
            index++;
            current = current.next;
        }
        TreeNode root = new TreeNode(current.val);
        TreeNode left = divideConquer(head, start, mid - 1);
        TreeNode right = divideConquer(head, mid + 1, end);
        root.left = left;
        root.right = right;
        return root;
    }
    
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) {
            return null;
        }
        int length = 0;
        ListNode current = head;
        while (current != null) {
            length++;
            current = current.next;
        }
        return divideConquer(head, 0, length - 1);
    }
}
```
__优化__  
right分支从mid.next开始  
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) {
            return null;
        }
        int length = getLength(head);
        return divideConquer(head, length - 1);
    }
    public TreeNode divideConquer(ListNode head, int end) {
        if (end < 0) {
            return null;
        }
        int mid = end / 2;
        int index = 0;
        ListNode current = head;
        while (index < end / 2) {
            index++;
            current = current.next;
        }
        TreeNode root = new TreeNode(current.val);
        TreeNode left = divideConquer(head, mid - 1);
        TreeNode right = divideConquer(current.next, end - mid - 1);
        root.left = left;
        root.right = right;
        return root;
    }
    public int getLength(ListNode head) {
        int length = 0;
        while (head != null) {
            length++;
            head = head.next;
        }
        return length;
    }
}
```
去掉参数start   
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {// start always == 0
    public TreeNode divideConquer(ListNode head, int end) {
        if (end < 0) {
            return null;
        }
        int mid = end / 2;
        int index = 0;
        ListNode current = head;
        while (index < mid) {
            index++;
            current = current.next;
        }
        TreeNode root = new TreeNode(current.val);
        TreeNode left = divideConquer(head, mid - 1);
        TreeNode right = divideConquer(current.next, end - mid - 1);
        root.left = left;
        root.right = right;
        return root;
    }
    
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) {
            return null;
        }
        int length = 0;
        ListNode current = head;
        while (current != null) {
            length++;
            current = current.next;
        }
        return divideConquer(head, length - 1);
    }
}
```

* * * 

#### 110. Balanced Binary Tree 

**Description**   
>Given a binary tree, determine if it is height-balanced.
>
>For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

**[题目链接](https://leetcode.com/problems/balanced-binary-tree/)**    

**Solution**   
**思路**  
递归。  
根据定义，平衡树在任意一个节点左右子树的高度相差不能超过1。递归从叶子节点往上判断是否每一个节点都是平衡的。若不平衡返回-1。  

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int maxDepth(TreeNode root) {
        //basis, or will never stop
        if (root == null) {
            return 0;
        }
        // recursive relations: 
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        if (left < 0 || right < 0 || Math.abs(left - right) > 1) {
            return -1;
        }
        return Math.max(left, right) + 1;
    }
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }
        return maxDepth(root) >= 0;
    }
}
```
* * *

#### 111. Minimum Depth of Binary Tree 

**Description**   
>Given a binary tree, find its minimum depth.
>
>The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**[题目链接](https://leetcode.com/problems/minimum-depth-of-binary-tree/)**    

**Solution**   
**思路1**  
递归。
recursive relations：  
root的minDepth取决于左右子树。  
f(root) = min(f(root.left), f(root.right)) + 1, left != null && right != null  
f(root) = f(left)+ 1, if left != null && right  == null  
f(root) = f(root.right) + 1, if left == null && right != null  
f(root) = 1, if left == null && right == null, leaf node  
basis:  
root == null, f(root) = 0  
**思路2**
level order traversal  

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        if (root.left != null && root.right != null) {
            return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
        } 
        if (root.left == null && root.right != null) {
            return minDepth(root.right) + 1;
        }
        if (root.left != null && root.right == null) {
            return minDepth(root.left) + 1;
        }
        return 1;
    }
}
```
BFS
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
 

public class Solution {
    public int minDepth(TreeNode root) {
        // bfs
        if (root == null) {
            return 0;
        }
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(root);
        int depth = 0;
        while(!queue.isEmpty()){
            int size= queue.size();
            depth++;
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (node.left == null && node.right == null) {
                    return depth;
                }
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return depth;
    }
}
```
* * *
#### 113. Path Sum II 

__Description__   
>Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.
>
>For example:
>Given the below binary tree and sum = 22,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
return
```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

[题目链接](https://leetcode.com/problems/path-sum-ii/)  

__Solution__  
**思路**  
DFS，path as 摆渡。   
叶子节点，sum == 0还是sum == root.val?    
if sum == 0会发生什么？如何判断root == null的情况？

**代码**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> result = new ArrayList();
        if (root == null) {
            return result;
        }
        List<Integer> path = new ArrayList();
        dfs(root, sum, 0, result, path);
        return result;
    }
    public void dfs(TreeNode root, int sum, int current, List<List<Integer>> result, List<Integer> path) {
        path.add(root.val);
        if (root.left == null && root.right == null) {
            if (current + root.val == sum) {
                result.add(new ArrayList(path));
            }
            path.remove(path.size() - 1);
            return;
        }
        if (root.left != null) {
            dfs(root.left, sum, current + root.val, result, path);
        }
        if (root.right != null) {
            dfs(root.right, sum, current + root.val, result, path);
        }
        path.remove(path.size() - 1);
    }
}
```
* * * 
### 114. Flatten Binary Tree to Linked List 

__Description__   
>Given a binary tree, flatten it to a linked list in-place.
>
>For example,
>Given
```
         1
        / \
       2   5
      / \   \
     3   4   6
 ```
The flattened tree should look like:
```
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

**[题目链接](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)**    

**Solution**   
**思路**  
题目中LinkedList的顺序与preorder traversal的顺序相同。因此用preorder traversal解决。  
注意：要将left pointer 置为null。

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        TreeNode prev = null;
        Stack<TreeNode> stack = new Stack();
        stack.push(root);
        while (!stack.empty()) {
            TreeNode node = stack.pop();
            if (prev != null) {
                prev.right = node;
            }
            prev = node;
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
            node.left = null; // **忘了将left child置为null
        }
    }
}
```
* * *
#### 116. Populating Next Right Pointers in Each Node 

**Description**   
>Given a binary tree
>```
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
>```
>Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

>Initially, all next pointers are set to NULL.
>
>Note:
>
>You may only use constant extra space.
>You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
>For example,
>Given the following perfect binary tree,
>```
         1
       /  \
      2    3
     / \  / \
    4  5  6  7
>```
>After calling your function, the tree should look like:
>```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
>```

**[题目链接](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)**    

**Solution**   
**思路**  
自上而下一次进行。用上层已经建立的next链接将以一层链接起来。水过~  

**代码**   
```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        if (root == null || root.left == null) {
            return;
        }
        TreeLinkNode head;
        while (root != null) {
            head = root;
            while (head != null) {
                if (head.left != null) {
                    head.left.next = head.right;
                }
                if (head.right != null && head.next != null) {
                    head.right.next = head.next.left;
                }
                head = head.next;
            }
            root = root.left;
        }
    }
}
```
* * *
#### 117. Populating Next Right Pointers in Each Node II 

**Description**   
>Follow up for problem "Populating Next Right Pointers in Each Node".
>
>What if the given tree could be any binary tree? Would your previous solution still work?
>
>Note:
>
>You may only use constant extra space.
>For example,
>Given the following binary tree,
```
         1
       /  \
      2    3
     / \    \
    4   5    7
```
>After calling your function, the tree should look like:
```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```

**[题目链接](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)**    

**Solution**   
**思路**  
与106题类似，不过需要注意的是，本题的树不是completed tree。因此需要一个prev指针存储每一层已经链接的部分，方便将下一个mode链接到prev。还需要注意的是，每一层的开头需要在下一层查找，并存为next，因为树的最左路径节点可能为空。

**代码**   
```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        if (root == null) {
            return;
        }
        TreeLinkNode prev; // prev of current level
        TreeLinkNode current = root; // head of current level
        TreeLinkNode next; // head of next level
        while (current != null) {
            prev = null;
            next = null;
            while (current != null) {
                if (current.left != null) {
                    if (prev != null) {
                        prev.next = current.left;
                    } else {
                        next = current.left;
                    }
                    prev = current.left;
                }
                if (current.right != null) {
                    if (prev != null) {
                        prev.next = current.right;
                    } else {
                        next = current.right;
                    }
                    prev = current.right;
                }
                current = current.next;
            }
            current = next;
        }
    }
}
```
* * *
#### 124. Binary Tree Maximum Path Sum 

**Description**   
>Given a binary tree, find the maximum path sum.
>
>For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path does not need to go through the root.
>
>For example:
>Given the below binary tree,
```
       1
      / \
     2   3
```
>Return 6.   

**[题目链接](https://leetcode.com/problems/binary-tree-maximum-path-sum/)**    

**Solution**   
**思路**  
Dvide and conquer，返回以root开头的maximum single path sum。
single path sum可能是max(max(left, right) + root.val, 0)，single可以为空。
则当前root下，maximum path sum可能是：
max(left maximun path, right maximum path, left single path + right single path + root.val)

**代码**   
single path可以是空，maxpath至少含一个节点。 
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public class Path{
        int singlePath;
        int maxPath;
        public Path(int singlePath, int maxPath) {
            this.singlePath = singlePath;
            this.maxPath = maxPath;
        }
    }
    public Path maxPath(TreeNode root) {
        if (root == null) {
            return new Path(0, Integer.MIN_VALUE);
        }
        Path left = maxPath(root.left);
        Path right = maxPath(root.right);
        int singlePath = Math.max(0, Math.max(left.singlePath, right.singlePath) + root.val);
        int maxPath = Math.max(left.singlePath + right.singlePath + root.val, Math.max(left.maxPath, right.maxPath));
        return new Path(singlePath, maxPath);
    }
    public int maxPathSum(TreeNode root) {
        return maxPath(root).maxPath;
    }
}
```
single path至少含一个节点，max path至少含一个节点。  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public class Path{
        int singlePath;
        int maxPath;
        public Path(int singlePath, int maxPath) {
            this.singlePath = singlePath;
            this.maxPath = maxPath;
        }
    }
    public Path maxPath(TreeNode root) {
        if (root == null) {
            return new Path(Integer.MIN_VALUE, Integer.MIN_VALUE);
        }
        Path left = maxPath(root.left);
        Path right = maxPath(root.right);
        int singlePath = Math.max(0, Math.max(left.singlePath, right.singlePath)) + root.val;
        int maxPath = Math.max(Math.max(left.singlePath, 0) + Math.max(right.singlePath, 0) + root.val, Math.max(left.maxPath, right.maxPath));
        return new Path(singlePath, maxPath);
    }
    public int maxPathSum(TreeNode root) {
        return maxPath(root).maxPath;
    }
}
```
Path class也可以用数组对象代替。
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int[] maxPath(TreeNode root) {
        int[] path = new int[2];
        // path[0]: max path
        // path[1]: single path
        if (root == null) {
            return new int[] {Integer.MIN_VALUE, 0};
        }
        int[] left = maxPath(root.left);
        int[] right = maxPath(root.right);
        
        path[1] = Math.max(0, Math.max(left[1], right[1]) + root.val);
        path[0] = Math.max(left[1] + right[1] + root.val, Math.max(left[0], right[0]));
        //System.out.prinn(path[0]);
        return path;
        
    }
    public int maxPathSum(TreeNode root) {
        return maxPath(root)[0];
    }
}
```
* * *
#### 129. Sum Root to Leaf Numbers

__Description__   
>Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.
>
>An example is the root-to-leaf path 1->2->3 which represents the number 123.
>
>Find the total sum of all root-to-leaf numbers.
>
>For example,
```
    1
   / \
  2   3
	```
>The root-to-leaf path 1->2 represents the number 12.
>The root-to-leaf path 1->3 represents the number 13.

>Return the sum = 12 + 13 = 25.

[题目链接](https://leetcode.com/problems/sum-root-to-leaf-numbers/)   


__Solution__    
**思路**   
直接DFS，Divide and conquer。将任务分派给left and right children，left and right到达goal node后返回目标，然后merge结果。  
__DFS__
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void dfs(TreeNode root, int pathSum, int[] sum) {
        if (root.left == null && root.right == null) {
            pathSum += root.val;
            sum[0] += pathSum;
            return;
        }
        pathSum += root.val;
        if (root.left != null) {
            dfs(root.left, pathSum * 10, sum);
        }
        if (root.right != null) {
            dfs(root.right, pathSum * 10, sum);
        }
    }
    public int sumNumbers(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int pathSum = 0;
        int[] sum = new int[1];
        sum[0] = 0;
        dfs(root, pathSum, sum);
        return sum[0];
    }
}
```
**divide and conquer**
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int dfs(TreeNode root, int sum) {
        if (root == null) {
            return 0;
        }
        sum = sum * 10 + root.val;
        if (root.left == null && root.right == null) {
            return sum;
        }
        return dfs(root.left, sum) + dfs(root.right, sum);
    }
    public int sumNumbers(TreeNode root) {
     return dfs(root, 0);   
    }
}
```
* * *
loop inveriant   
preorder traversal, 到达current node后, 访问之, 然后先访问left child再访问right child, so 
```
TreeNode current = stack.pop();
result.add(current.val);
if (current.right != null) {
    stack.push(current.right);
}
if (current.left != null) {
    stack.push(current.left);
}
```
postorder traversal,到达current node后，先判定children是否已经被访问, 若被访问, 访问current node, 然后go up, else 要先访问children  
```
TreeNode current = stack.peek();
if (current.left == null && current.right == null ||  
    prev == current.right || prev == current.right) {
    stack.pop();
    result.add(current.val);
    prev = current;
} else {
    // same as preorder traversal
}
```
inorder traversal, 到达current node节点后, 要访问right child再访问right
```
while (current != null) {
    stack.push(current);
    current = current.left;
}
current = stack.pop();
result.add(current.val);
current = current.right;
```
* * *

#### 144. Binary Tree Preorder Traversal 

**Description**   
>Given a binary tree, return the preorder traversal of its nodes' values.
>
>For example:
>Given binary tree {1,#,2,3},
```
   1
    \
     2
    /
   3
```
>return [1,2,3].

**[题目链接](https://leetcode.com/problems/binary-tree-preorder-traversal/)**    

**Solution**   
**思路**  
Recursive and iterative.  

**代码**   
**Iterative**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
import java.util.*;
public class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer> ();
        Stack<TreeNode> stack = new Stack<TreeNode> ();
        
        if (root == null) {
            return result;
        }
        
        stack.push(root);
        
        while (!stack.empty()) {
            TreeNode current = stack.pop();
            result.add(current.val);
            if (current.right != null) {
                stack.push(current.right);
            }
            if (current.left != null) {
                stack.push(current.left);
            }
        }
        return result;
    }
}
```
**Recursive**
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void preorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        result.add(root.val);
        preorder(root.left, result);
        preorder(root.right, result);
    }
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        preorder(root, result);
        return result;
    }
}
```
* * *

#### 145. Binary Tree Postorder Traversal 

**Description**   
>Given a binary tree, return the postorder traversal of its nodes' values.
>
>For example:
>Given binary tree {1,#,2,3},
```
   1
    \
     2
    /
   3
```	 
>return [3,2,1].

**[题目链接](https://leetcode.com/problems/binary-tree-postorder-traversal/)**    

**Solution**   
**思路**  
修改：  
Binary Tree的preorder and postorder tranversal应该是类似的，之前的思路逻辑性太差了。   
套用preorder的模板，但在将current.val加入result之前，先检验是否已经访问children。  

prev可能的三种情况。
* right child, if current.right != null
* left child, if current.right == null && current.left != null
* ancestor.left, ancecstor is the nearest ancestor whoes left child != null  
左右子树没有访问，要先访问children入栈。均被访问，pop()

**代码**   
**Iterative**
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList();
        if (root == null) {
            return result;
        }
        
        Stack<TreeNode> stack = new Stack();
        stack.push(root);
        TreeNode prev = new TreeNode(-1);
        while (!stack.empty()) {
            TreeNode current = stack.peek();
            // if all children are visited
            if (current.left == null && current.right == null ||
                prev == current.right || prev == current.left) {
                // current has no children or back from children
                stack.pop();
                result.add(current.val);
                prev = current;
            } else {
                // children are not visited
                if (current.right != null) {
                    stack.push(current.right);
                }
                if (current.left != null) {
                    stack.push(current.left);
                }
            }
        }
        return result;
    }
}
```
**Iterative**
**思路1. 后续遍历每个节点被访问两次，第一次left and right没有被访问，第二次是left和right都已经被访问。而且要再第二次重新返回到节点时将节点加入到结果中。**
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        TreeNode prev = null;
        while (!stack.empty()) {
            TreeNode current = stack.peek();
            if (current.right == null && current.left == null || 
                prev != null && (prev == current.right || prev == current.left)) {
                // back from child
                stack.pop();
                result.add(current.val);
                prev = current;
            } else {
                if (current.right != null) {
                    stack.push(current.right);
                }
                if (current.left != null) {
                    stack.push(current.left);
                }
            }
        }
        return result;
    }
}
```
**思路2. 将遍历过程分为从parent到child，从left返回，从right返回。从left返回后遍历right，从right返回后将节点pop.**
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        ArrayList<Integer> list = new ArrayList<Integer> ();
        Stack<TreeNode> stack = new Stack<TreeNode> ();
        if (root == null) return list;
        
        TreeNode prev = new TreeNode (-1);
        prev.left = root;
        TreeNode current = root;
        stack.add(root);
        
        while (!stack.empty()) {
            current = stack.peek();
            if (prev != null && (prev.left == current || prev.right == current)) {
                // come from parent
                if (current.left != null) {
                    stack.add(current.left);
                    prev = current;
                } else {
                    prev = null;
                }
            } else if (current.left == prev) {
                // come back from left
                if (current.right != null) {
                    stack.add(current.right);
                } 
                prev = current;
            } else  {
                // come back from right
                list.add(current.val);
                stack.pop();
                prev = current;
            }
        }
        return list;
    }
}
```
**Recursive**
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void postorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        postorder(root.left, result);
        postorder(root.right, result);
        result.add(root.val);
    }
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        postorder(root, result);
        return result;
    }
}
```
* * *
#### 173. Binary Search Tree Iterator 

**Description**   
>Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.
>
>Calling next() will return the next smallest number in the BST.
>
>Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.
>
>**[题目链接](https://leetcode.com/problems/binary-search-tree-iterator/)**    


**Solution**   
**思路**  
Inorder traversal of binary tree.  水过~

**代码**   
```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

public class BSTIterator {
    private Stack<TreeNode> stack;
    public BSTIterator(TreeNode root) {
        stack = new Stack<TreeNode>();
        while (root != null) {
            stack.push(root);
            root = root.left;
        }
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.empty();
        
    }

    /** @return the next smallest number */
    public int next() {
        if (hasNext()) {
            TreeNode next = stack.pop();
            TreeNode current = next.right;
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            return next.val;
        }
        return -1;
    }
}

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = new BSTIterator(root);
 * while (i.hasNext()) v[f()] = i.next();
 */
```
* * *
#### 199. Binary Tree Right Side View

**Description**   
>Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.
>
>For example:
>Given the following binary tree,
```
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```
>You should return [1, 3, 4].


**[题目链接]()**    


**Solution**   
**思路**  
思路1： level order traversal。将level的最后一个元素加入到result list。比较简单。  
思路2： dfs，前序遍历，需要two stacs, first stack stores treeNodes, second stack stores depth of nodes, 入栈时，先左后右。

**代码**
**BFS**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> list = new ArrayList();
        if (root == null) {
            return list;
        }

        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i=0; i<size; i++) {
                TreeNode node = queue.poll();
                if (i == size - 1) {
                    list.add(node.val);
                }
                if(node.left != null) {
                    queue.add(node.left);
                }
                if (node.right != null) {
                    queue.add(node.right);
                }
            }
        }
        return list;
    }
}
```

**DFS 非递归**
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        Stack<TreeNode> stack = new Stack<TreeNode>();
        Stack<Integer> depthStack = new Stack<Integer>();
        int maxDepth = 0;
        
        stack.push(root);
        depthStack.push(1);
        while (!stack.empty()) {
            TreeNode current = stack.pop();
            int depth = depthStack.pop();
            if (depth > maxDepth) {
                result.add(current.val);
            }
            maxDepth = Math.max(maxDepth, depth);
            if (current.left != null) {
                stack.push(current.left);
                depthStack.push(depth + 1);
            }
            if (current.right != null) {
                stack.push(current.right);
                depthStack.push(depth + 1);
            }
        }
        return result;
    }
}
```
* * *
#### 222. Count Complete Tree Nodes 

**Description**   
>Given a complete binary tree, count the number of nodes.
>
>Definition of a complete binary tree from Wikipedia:
>In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

**[题目链接](https://leetcode.com/problems/count-complete-tree-nodes/)**    


**Solution**   
**思路**  
递归+二分查找。每次叶子节点二分，看mid的深度是否与左路径深度相等，若相等O(1)计算出左子树的节点数目并discard，若不等，O(1)计算出右子树的节点数目并discard。  
也可非递归，复杂度均为O(logn * log n)。
```
if heightL == heightR
	numberNodes = (1 << heightR - 1 +1) + countNodes(root.right)
else 
	numberNodes = (1 << heightR - 1 + 1) + countNodes(root.left)
```


**代码**   
**递归 + 二分查找**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int getHeight(TreeNode root) {
        int height = 0;
        while (root != null) {
            height++;
            root = root.left;
        }
        return height;
    }
    public int countNodesHelper(TreeNode root, int height) {
       if (root == null) {
           return 0;
       }
       int heightR = getHeight(root.right);
       int num = (1 << heightR);
       if (heightR == height - 1) {
           // go right
           return num + countNodesHelper(root.right, height - 1);
       } else {
           // go left;
           return num + countNodesHelper(root.left, height - 1);
       }
    }
    public int countNodes(TreeNode root) {
       int height = getHeight(root);
       return countNodesHelper(root, height);
    }
}
```
**非递归，及优化**   
**左子树的高度不必要每一层都求取，可以初始化时求一次，然后逐层递减，对比一下代码**
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int getHeight(TreeNode root) {
        int height = 0;
        while (root != null) {
            height++;
            root = root.left;
        }
        return height;
    }
   
    public int countNodes(TreeNode root) {
       if (root == null) {
           return 0;
       }
       int sum = 0;
       int hl = 0;
       int hr = 0;
       while (root != null) {
           hl = getHeight(root.left);
           hr = getHeight(root.right);
           if (hl == hr) {
               sum += (1 << hr);
               root = root.right;
           } else {
               sum += (1 << hr);
               root = root.left;
           }
       }
       return sum;
    }
}
```
优化后
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int getHeight(TreeNode root) {
        int height = 0;
        while (root != null) {
            height++;
            root = root.left;
        }
        return height;
    }
   
    public int countNodes(TreeNode root) {
       if (root == null) {
           return 0;
       }
       int sum = 0;
       int hl = 0;
       int hr = 0;
       while (root != null) {
           hl = getHeight(root.left);
           hr = getHeight(root.right);
           if (hl == hr) {
               sum += (1 << hr);
               root = root.right;
           } else {
               sum += (1 << hr);
               root = root.left;
           }
       }
       return sum;
    }
}
```
* * *
#### 226. Invert Binary Tree 

**Description**   
>Invert a binary tree.
```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```
to
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**[题目链接](https://leetcode.com/problems/invert-binary-tree/)**    


**Solution**   
**思路**  
dfs或者divide and conquer。

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return root;
        }
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
}
```
非递归 
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return root;
        }
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        while (!stack.empty()) {
            TreeNode current = stack.pop();
            TreeNode temp = current.left;
            current.left = current.right;
            current.right = temp;
            if (current.left != null) {
                stack.push(current.left);
            }
            if (current.right != null) {
                stack.push(current.right);
            }
        }
        return root;
    }
}
```
* * *
#### 230. Kth Smallest Element in a BST 

**Description**   
>Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.
>
>Note: 
>You may assume k is always valid, 1 ≤ k ≤ BST's total elements.
>
>Follow up:
>What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?
>
>Hint:
>
>Try to utilize the property of a BST.
>What if you could modify the BST node's structure?
>The optimal runtime complexity is O(height of BST).

**[题目链接](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)**    

**Solution**   
**思路**  
思路1：直接inorder traversal，list.size() == k，返回。
思路2： binary search.

**代码**   
**Inorder Traversal**  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int kthSmallest(TreeNode root, int k) {
        // inorder traverse
        ArrayList<Integer> list = new ArrayList<Integer>();
        Stack<TreeNode> stack = new Stack<TreeNode> ();
        
        TreeNode current = root;
        while ( current != null || !stack.empty()) {
            while (current != null) {
                stack.add(current);
                current = current.left;
            }
            current = stack.pop();
            list.add(current.val);
            current = current.right;
            if (list.size() == k) {
                return list.get(k-1);
            }
        }
        return -1;
    }
}
```
**Binary Search**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return 1 + countNodes(root.left) + countNodes(root.right);
    }
    public int kthSmallest(TreeNode root, int k) {
        int count = countNodes(root.left);
        if (count + 1 == k) {
            return root.val;
        }
        if (count + 1 < k) {
            return kthSmallest(root.right, k - count - 1);
        } else {
            return kthSmallest(root.left, k);
        }
    }
}
```
* * *
#### 235. Lowest Common Ancestor of a Binary Search Tree 

**Description**   
>Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.
>
>According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

```

        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5
``` 
>For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.


**[题目链接](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)**    


**Solution**   
**思路**  
dfs。recursive relations：  
at root - left and right。
if left == null return right; 
if right == null return left; 
if left != null && right != null return root;  

**代码**   
仅限于BST的解法。  
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root.val > p.val && root.val > q.val) {
            return lowestCommonAncestor(root.left, p, q);
        } else if (root.val < p.val && root.val < q.val) {
            return lowestCommonAncestor(root.right, p, q);
        } else {
            return root;
        }
    }
}
```
通用解法。
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            return root;
        }
        // divie
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        
        // comquer
        if (left != null && right != null) {
            return root;
        }
        if (left != null) {
            return left;
        }
        if (right != null) {
            return right;
        }
        return null;
    }
}
```
* * *

#### 236. Lowest Common Ancestor of a Binary Tree 

**Description**   
>Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.
>
>According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

```
        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
```
>For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

**[题目链接](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)**    

**Solution**   
**思路**  
与235类似。  
dfs。recursive relations：  
at root - left and right。
if left == null return right; 
if right == null return left; 
if left != null && right != null return root;  

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // basis case, violate the rule
        if (root == null || root == p || root == q) {
            return root;
        }
        // recursive relation
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) {
            return root;
        }
        if (left != null) {
            return left;
        }
        if (right != null) {
            return right;
        }
        return null;
    }
}
```
* * *
#### 257. Binary Tree Paths 

**Description**   
>Given a binary tree, return all root-to-leaf paths.
>
>For example, given the following binary tree:
```
   1
 /   \
2     3
 \
  5
```
>All root-to-leaf paths are:
>
`["1->2->5", "1->3"]`

**[题目链接](https://leetcode.com/problems/binary-tree-paths/)**    


**Solution**   
**思路**  
dfs，不返回值。或divide and conquer返回List<String>然后merge。  

**代码**   
**DFS**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void dfs(TreeNode root, List<String> result, List<Integer> path) {
        // basis
        if (root.left == null && root.right == null) {
            StringBuilder sb = new StringBuilder();
            for (int i : path) {
                sb.append(i);
                sb.append("->");
            }
            sb.append(root.val);
            result.add(sb.toString());
            return;
        }
        // recursive relations
        path.add(root.val);
        if (root.left != null) {
            dfs(root.left, result, path);
        }
        if (root.right != null) {
            dfs(root.right, result, path);
        }
        path.remove(path.size() - 1);
    }
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<String>();
        if (root == null) {
            return result;
        }
        List<Integer> path = new ArrayList<Integer>();
        dfs(root, result, path);
        return result;
    }
}
```

**Divide and Counquer**    
 ```java
 /**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> list = new ArrayList<String>();
        if(root == null) return list;
        if(root.left == null && root.right == null){
            list.add("" + root.val);
            return list;
        }
      
        if(root.left != null) {
            List<String> left = binaryTreePaths(root.left);
            for(int i = 0; i < left.size(); i ++){
                left.set(i, root.val + "->" + left.get(i));
            }
            list.addAll(left);
        }
        if(root.right != null){
            List<String> right = binaryTreePaths(root.right);
            for(int i = 0; i < right.size(); i ++){
                right.set(i, root.val + "->" + right.get(i));
            }
            list.addAll(right);
        }
        return list;
    }
}
 ```
* * *
#### 297. Serialize and Deserialize Binary Tree 

**Description**   
>Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.
>
>Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.
>
>For example, you may serialize the following tree
```
    1
   / \
  2   3
     / \
    4   5
```
>as "[1,2,3,null,null,4,5]", just the same as how LeetCode OJ serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.
>Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

**[题目链接](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)**    

**Solution**   
**思路**  
BFS。序列化是用level order traversal。反序列化是同样用level order traversal，要反过来用，用index判断是否poll出queue head.  

**代码**   
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode current = queue.poll();
            if (current != null) {
                sb.append(current.val + ",");
                queue.offer(current.left);
                queue.offer(current.right);
            } else {
                sb.append("null,");
            }
        }
        return sb.substring(0, sb.length() - 1);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] array = data.split(",");
        TreeNode root = null;
        if (array[0].equals("null")) {
            return root;
        } else {
            root = new TreeNode(Integer.parseInt(array[0]));
        }
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        for (int i = 1; i < array.length; i++) {
            TreeNode current = queue.peek();
            TreeNode child = null;
            if (!array[i].equals("null")) {
                
                child = new TreeNode(Integer.parseInt(array[i]));
                queue.offer(child);
                if (i % 2 == 1) {
                    current.left = child; 
                } else {
                    current.right = child;
                }
            }
            if (i % 2 == 0) {
                queue.poll();
            }
        }
        queue.clear();
        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```
* * *
