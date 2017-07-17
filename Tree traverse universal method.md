#### Universal method to preorder, inorder and postorder traverse of Binary Tree
首先，使用递归很容易实现Binary Tree的三种遍历，而且代码几乎相同，仅仅更改了访问parent node和left and child nodes的顺序。
```java
// preorder
visit parent;
visit left;
visit right

// inorder
visit left;
visit parent;
visit right;

//postorder
visit left;
visit right;
visit parent;
```

但是，非递归方法，三种遍历的代码逻辑差别很大，很违反直觉。
```
stack.push(root);
//preorder
while(!stack.empty()){
	currNode = stack.pop();
	visit currNode;
	if (right child != null) {
		stack.push(right);
	}
	if (left.child != null) {
		stack.push(left);
	}
}

// inorder
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

//postorder
TreeNode current = root;
TreeNode prev = null;
while (current != null || !stack.empty()) {
	while (current != null) {
		stack.push(current);
		current = current.left;
	}
	current  = stack.peek();
	// check if current.right has been visited or not
	if (current.right == null || current.right == prev) {
		// visit current node
		result.add(current.val);
		stack.pop();
		prev = current;
		current = null; // indicating current subtree has been traversed
	} else { 
		// if current right has not been visited yet, start to traverse right subtree
		current = current.right;   
	}
}
```

这是因为，上述方法是用stack模拟的仅仅是节点的访问顺序，而非代码的执行过程。在代码的执行过程中，递归调用时，要将context全部入栈，包括参数和Program Counter(PC)，PC记录了当前context的下一条指令。要模拟递归程序的调用过程，要将context全部入栈，可以用一个数组存储context，并用一个Integer作为PC。
```
stack
postorder:　0-left, 1-right, 2-current
-------
[current], 0
------
[current], 1
[current], 0
------
[current], 1
[current], 1
[current], 0 // left and right === null
------
[current], 1
[current], 2
[current], 0 // left and right === null
------
[current], 1
[current], 2 // visit current and pop from stack
------
[current], 2
[current], 0
.
.
.

```

对于本问题，可以不用存储数组和PC,用current.val, current.left, current.right模拟context, 用入栈顺序模拟PC。
#### preorder

```
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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();
        if (root == null) {
            return list;
        }
        Stack<Object> stack = new Stack<Object>();
        stack.push(root);
        
        // universal method, could be optimized 
        while (!stack.empty()) {
            Object curr = stack.pop();
            if (curr instanceof TreeNode) {
                TreeNode currNode = (TreeNode)curr;
                if (currNode.right != null) {
                    stack.push(currNode.right);
                }
                if (currNode.left != null) {
                    stack.push(currNode.left);
                }
                stack.push(currNode.val); // push Integer
            } else {
                list.add((Integer)curr);
            }
        }
        return list;
        
    }
}
```

#### inorder
```
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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<Integer>();
        if (root == null) {
            return list;
        }
        
        Stack<Object> stack = new Stack<Object>();
        stack.push(root);
        while (!stack.empty()) {
            Object curr = stack.pop();
            if (curr instanceof TreeNode) {
                TreeNode currNode = (TreeNode)curr;
                if (currNode.right != null) {
                    stack.push(currNode.right);
                }
                stack.push(new Integer(currNode.val));
                if (currNode.left != null) {
                    stack.push(currNode.left);
                }
            } else {
                list.add((Integer)curr);
            }
        }
        
        return list;
    }
    
    
}
```

#### postorder
```
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
        List<Integer> list = new ArrayList<Integer>();
        if (root == null) {
            return list;
        }
        Stack<Object> stack = new Stack<Object>();
        stack.push(root);
        while(!stack.empty()) {
            Object curr = stack.pop();
            if (curr instanceof TreeNode) {
                TreeNode currNode = (TreeNode)curr;
                stack.push(currNode.val);
                if (currNode.right != null) {
                    stack.push(currNode.right);
                }
                if (currNode.left != null) {
                    stack.push(currNode.left);
                }
            } else {
                list.add((Integer)curr);
            }
        }
        return list;
        
    }
 
}
```
