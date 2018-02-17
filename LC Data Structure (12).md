146 LRU Cache  
150 Evaluate Reverse Polish Notation  
155 Min Stack  
187 Repeated DNA Sequences  
208 Implement Trie (Prefix Tree)    
211 Add and Search Word – Data structure design  
212. Word Search II  
218 The Skyline Problem   
232 Implement Queue using Stacks  
239 Sliding Window Maximum  
341 Flatten Nested List Iterator  
352 Data Stream as Disjoint Intervals  

* * *  
#### 146. LRU Cache 

**Description**   
>Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and set.
>
>get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
>set(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

**[题目链接](https://leetcode.com/problems/lru-cache/)**    

**Solution**   
**思路**   
key可能是内存地址，value可能是object。  
用一个FIFO队列。  
每次访问节点，将节点移动到队尾。  
每次新建节点，将节点加到队尾。   
若队列size > capacity，将队头poll出。  

由于java的queue不能remove节点，自己实现一个双向链表，采用sentinel的方式实现。

**代码**   
```java
public class LRUCache {
    class ListNode {
        int key;
        int value;
        ListNode prev;
        ListNode next;
        public ListNode(int key, int value) {
            this.key = key;
            this.value = value;
            this.prev = null;
            this.next = null;
        }
    }
    class List {
        ListNode head;
        int size;
        public List() {
            head = new ListNode(-1, -1);
            head.next = head;
            head.prev = head;
            size = 0;
        }
        public void offer(ListNode node) {
            node.next = head;
            node.prev = head.prev;
            head.prev.next = node;
            head.prev = node;
            size++;
        }
        public void remove(ListNode node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
            size--;
        }
        public ListNode poll() {
            ListNode node = head.next;
            if (node == head) {
                return null;
            }
            remove(node);
            return node;
        }
        public int size() {
            return size;
        }
    }
    
    private List queue;
    private HashMap<Integer, ListNode> hm;
    private int capacity;
    public LRUCache (int capacity) {
        this.capacity = capacity;
        queue = new List();
        hm = new HashMap<Integer, ListNode>();
    }
    
    public int get(int key) {
        if (hm.containsKey(key)) {
            ListNode n = hm.get(key);
            queue.remove(n);
            queue.offer(n);
            return n.value;
        }
        return -1;
    }
    
    public void set(int key, int value) {
        if (!hm.containsKey(key)) {
            ListNode n = new ListNode(key, value);
            if (hm.size() == capacity) {
                ListNode node = queue.poll();
                hm.remove(node.key);
            } 
            queue.offer(n);
            hm.put(key, n);
       
        } else {
            ListNode n = hm.get(key);
            n.value = value;
            queue.remove(n);
            queue.offer(n);
        }
        
    }
}
```
* * *
#### 150. Evaluate Reverse Polish Notation 

**Description**   
>Evaluate the value of an arithmetic expression in Reverse Polish Notation.
>
>Valid operators are +, -, *, /. Each operand may be an integer or another expression.
>
>Some examples:   
```
  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6
``` 

**[题目链接](https://leetcode.com/problems/evaluate-reverse-polish-notation/)**    

**Solution**   
**思路**  
用stack。
若当前string是运算符，pop出来两个数，将两个数的运算结果push到stack中。注意两个数的顺序与出栈顺序相反。
若当前string为numeric，直接入栈。

**代码**   
```java
public class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<Integer>();
        for (int i = 0; i < tokens.length; i++) {
            char c = tokens[i].charAt(tokens[i].length() - 1);
            if (Character.isDigit(c)) {
                stack.push(Integer.parseInt(tokens[i]));
            } else {
                if (stack.size() >= 2) {
                    int num2 = stack.pop();
                    int num1 = stack.pop();
                    if (c == '+') {
                        num1 += num2;
                    } else if (c == '-') {
                        num1 -= num2;
                    } else if (c == '*') {
                        num1 *= num2;
                    } else {
                        num1 /= num2;
                    }
                    stack.push(num1);
                }
            } 
        }
        if (stack.size() == 1) {
            return stack.pop();
        }
        return 0;
    }
}
```
* * *

#### 155. Min Stack 

**Description**   
>Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
>
>push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.
Example:
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.

**[题目链接](https://leetcode.com/problems/min-stack/)**    

**Solution**   
**思路**  
双栈，用一个minStack维护到当前位置的最小值。 
For comming node, push the min from start to comming (including) to minStack.   
**注意**：pop并不是pop除当前的最小值，而是stack中的栈顶元素。  

**代码**   
```java
public class MinStack {
    private Stack<Integer> stack;
    private Stack<Integer> minStack;
    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack();
        minStack = new Stack();
    }
    
    public void push(int x) {
        stack.push(x);
        if (minStack.empty()) {
            minStack.push(x);
        } else {
            minStack.push(Math.min(minStack.peek(), x));
        }
    }
    
    public void pop() {
        minStack.pop();
        stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```
* * *
#### 187. Repeated DNA Sequences 

**Description**   
>All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.
>
>Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.
>
>For example,
```
Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",
Return:
["AAAAACCCCC", "CCCCCAAAAA"].
```

**[题目链接](https://leetcode.com/problems/repeated-dna-sequences/)**    

**Solution**   
**思路**  
枚举所有长度为10的子串，用HashSet查重。  

**代码**   
```java
public class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        HashSet<String> re = new HashSet<String>();
        HashSet<String> hs = new HashSet<String>();
        
        for (int i = 0; i < s.length() - 9; i++) {
            String sub = s.substring(i, i + 10);
            if (hs.contains(sub)) {
                re.add(sub);
            } else {
                hs.add(sub);
            }
        }
        List<String> result = new ArrayList<String>();
        for (String str : re) {
            result.add(str);
        }
        return result;
    }
}
```
* * *

#### 208. Implement Trie (Prefix Tree) 

**Description**   
>Implement a trie with insert, search, and startsWith methods.
>
>Note:
>You may assume that all inputs are consist of lowercase letters a-z.

**[题目链接](https://leetcode.com/problems/implement-trie-prefix-tree/)**    

**Solution**   
**思路**  
类似于tree的插入和查找操作。需要注意的是，在每个节点需要一个end变量表明在此节点是否有一个word。  

**代码**   
```java
class TrieNode {
    // Initialize your data structure here.
    Character c;
    HashMap<Character, TrieNode> children;
    boolean hasWord; // check if there is a word in this node
    public TrieNode() {
        this.children = new HashMap();
    }
    public TrieNode(Character c) {
        this.c = c;
        this.children = new HashMap();
        this.hasWord = false;
    }
}

public class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        TrieNode current = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            HashMap<Character, TrieNode> children = current.children;
            if (children.containsKey(c)) {
                current = children.get(c);
            } else {
                TrieNode node = new TrieNode(c);
                children.put(c, node);
                current = node;
            }
        }
        current.hasWord = true;
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
       TrieNode node = matchWord(word);
       return node != null && node.hasWord;
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
       TrieNode node = matchWord(prefix);
       return node != null;
    }
    public TrieNode matchWord(String word) {
        TrieNode current = root;
        int i = 0;
        HashMap<Character, TrieNode> children;
        while (current != null && i < word.length()) {
            char c = word.charAt(i);
            children = current.children;
            current = children.get(c);
            i++;
        }
        return current;
    }
}

// Your Trie object will be instantiated and called as such:
// Trie trie = new Trie();
// trie.insert("somestring");
// trie.search("key");
```
* * *
#### 211. Add and Search Word - Data structure design 

**Description**   
>Design a data structure that supports the following two operations:
```
void addWord(word)
bool search(word)
```
>search(word) can search a literal word or a regular expression string containing only letters a-z or .. A . means it can represent any one letter.

>For example:
```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

**[题目链接](https://leetcode.com/problems/add-and-search-word-data-structure-design/)**    

**Solution**   
**思路**  
与208一样，查找的时候对c == '.'要遍历当前node的所有children。用DFS。  

**代码**   
```java
class TrieNode {
    // Initialize your data structure here.
    Character c;
    HashMap<Character, TrieNode> children;
    boolean hasWord; // check if there is a word in this node
    public TrieNode() {
        this.children = new HashMap();
        this.hasWord = false;
    }
    public TrieNode(Character c) {
        this.c = c;
        this.children = new HashMap();
        this.hasWord = false;
    }
}

public class WordDictionary {
    private TrieNode root;
    
    public WordDictionary() {
        root = new TrieNode();
    }

    // Adds a word into the data structure.
    public void addWord(String word) {
        TrieNode current = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            HashMap<Character, TrieNode> children = current.children;
            if (children.containsKey(c)) {
                current = children.get(c);
            } else {
                TrieNode node = new TrieNode(c);
                children.put(c, node);
                current = node;
            }
        }
        current.hasWord = true;
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        return matchWord(word, root, 0);
    }
    public boolean matchWord(String word, TrieNode root, int pos) {
        if (root == null || pos > word.length()) {
            return false;
        }    
        if (pos == word.length()) {
            return root.hasWord;
        }
        char c = word.charAt(pos);
        if (c == '.') {
            for (char n : root.children.keySet()) {
                if (matchWord(word, root.children.get(n), pos + 1)) {
                    return true;
                }
            }
            return false;
        } else {
            if (root.children.get(c) != null) {
                return matchWord(word, root.children.get(c), pos + 1);
            }
            return false;
        }
    }
}

// Your WordDictionary object will be instantiated and called as such:
// WordDictionary wordDictionary = new WordDictionary();
// wordDictionary.addWord("word");
// wordDictionary.search("pattern");
```
* * *
#### 212. Word Search II 

**Description**   
>Given a 2D board and a list of words from the dictionary, find all words in the board.
>
>Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
>
>For example,
>Given words = ["oath","pea","eat","rain"] and board =
```
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
````
>Return ["eat","oath"].

**[题目链接](https://leetcode.com/problems/word-search-ii/)**    

**Solution**   
**思路**  
此题与word search I都是用dfs (backtracking)解决。本题要求提高效率，用trie实现，将要查找的所有word放到trie中，这样dfs是每个节点不用再遍历所有word，而只要在trie中查找就好了。
第一次写时，老是报错，找不到原因，后来发现在将word加入到list后直接返回了。。。返回了。。。为啥要返回啊！！！！！！ :scream::scream::scream::scream:

**代码**   
```java
public class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        
        List<String> result = new ArrayList<String>();
        if (board == null || board.length == 0 || board[0] == null || board[0].length == 0) {
            return result;
        }
        if (words == null || words.length == 0) {
            return result;
        }
        TrieNode root = buildTrie(words);
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                dfs(board, root, i, j, result);
            }
        }
        return result;
    }
    public void dfs(char[][] board, TrieNode p, int i, int j, List<String> result) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) {
            return;
        }
        
        char c = board[i][j];
        if (c == '#' || p.next[c - 'a'] == null) {
            return;
        }
        int index = c - 'a';
        TrieNode next = p.next[c - 'a'];
        if (next.word != null) {
            result.add(next.word);
            next.word = null;
            //**************************************
            // DON'T RETURN HERE!!!!
            //return;
            //**************************************
        }
        board[i][j] = '#';
        dfs(board, next, i - 1, j, result);
        dfs(board, next, i + 1, j, result);
        dfs(board, next, i, j - 1, result);
        dfs(board, next, i, j + 1, result);
        board[i][j] = c;
    }
    // build trie part
    public TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        for (String word : words) {
            TrieNode current = root;
            for (char c : word.toCharArray()) {
                int index = c - 'a';
                if (current.next[index] == null) {
                    current.next[index] = new TrieNode();
                }
                current = current.next[index];
            }
            current.word = word;
        }
        return root;
    }
    // Trie part
    class TrieNode {
        TrieNode[] next = new TrieNode[26];
        String word;
    }
}
```
* * *
#### 218. The Skyline Problem 

**Description**   
>A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are given the locations and height of all the buildings as shown on a cityscape photo (Figure A), write a program to output the skyline formed by these buildings collectively (Figure B).  
<img src="https://leetcode.com/static/images/problemset/skyline1.jpg" style="width:100">

>
>The geometric information of each building is represented by a triplet of integers [Li, Ri, Hi], where Li and Ri are the x coordinates of the left and right edge of the ith building, respectively, and Hi is its height. It is guaranteed that 0 ≤ Li, Ri ≤ INT_MAX, 0 < Hi ≤ INT_MAX, and Ri - Li > 0. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

>For instance, the dimensions of all buildings in Figure A are recorded as: [ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ] .
>
>The output is a list of "key points" (red dots in Figure B) in the format of [ [x1,y1], [x2, y2], [x3, y3], ... ] that uniquely defines a skyline. A key point is the left endpoint of a horizontal line segment. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.
>
>For instance, the skyline in Figure B should be represented as:[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ].
>
>Notes:
>
>The number of buildings in any input list is guaranteed to be in the range [0, 10000].
>The input list is already sorted in ascending order by the left x position Li.
>The output list must be sorted by the x position.
>There must be no consecutive horizontal lines of equal height in the output skyline. For instance, [...[2 3], [4 5], [7 5], [11 5], [12 7]...] is not acceptable; the three lines of height 5 should be merged into one in the final output as such: [...[2 3], [4 5], [12 7], ...]


**[题目链接](https://leetcode.com/problems/the-skyline-problem/)**    

**Solution**   
**思路**  
直观方法，将rectangle的高度映射到x轴上，然后scan from left to right，高度变化的点就是要找的critical point。时间O(n^2)，空间左右rectangle的距离差。  
改进：   
**[非常详细的解释](https://briangordon.github.io/2014/08/the-skyline-problem.html)**   
所有关键点都可能出现在最后的result的list中，从左到右扫描关键点，判定是否将该关键点加入到result list中。
将关键点排序，因为关键点的x可能重复，排序按照：
1. x大小。
2. start 还是end point。
3. start时，height先大后小。
4. end时，height先小后大。 
维护一个heap存已经遍历的关键点的height。add时从最大的开始，remove时从最小的开始。


**代码**   
```java
public class Solution {
    class PointComparator implements Comparator<int[]> {
        @Override 
        public int compare(int[] a, int[] b) {
            if (a[0] != b[0]) {
                return a[0] - b[0];
            } else if (a[2] != b[2]) {
                return a[2] - b[2];
            } else if (a[2] == 1) {
                return a[1] - b[1];
            } else {
                return b[1] - a[1];
            }
        }
    }
    public List<int[]> getSkyline(int[][] buildings) {
        List<int[]> result = new ArrayList<int[]>();
        if (buildings == null || buildings.length == 0 || buildings[0] == null || buildings[0].length == 0) {
            return result;
        }
        List<int[]> cps = new ArrayList<int[]>();
        for (int[] b : buildings) {
            cps.add(new int[]{b[0], b[2], 0});
            cps.add(new int[]{b[1], b[2], 1});
        }
        Collections.sort(cps, new PointComparator());
        PriorityQueue<Integer> heights = new PriorityQueue<Integer>(10, Collections.reverseOrder());
        for (int[] p : cps) {
            if (p[2] == 0) {
                if (heights.isEmpty() || p[1] > heights.peek()) {
                    result.add(new int[]{p[0], p[1]});
                }
                heights.add(p[1]);
            } else {
                heights.remove(p[1]);
                if (heights.isEmpty() || p[1] > heights.peek()) {
                    result.add(new int[] {p[0], heights.isEmpty() ? 0 : heights.peek()});
                } 
            }
        }
        return result;
    }
}
```
* * *
#### 232. Implement Queue using Stacks 

**Description**   
>Implement the following operations of a queue using stacks.
>
>- push(x) -- Push element x to the back of queue.
>- pop() -- Removes the element from in front of queue.
>- peek() -- Get the front element.
>- empty() -- Return whether the queue is empty.

>Notes:
>- You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
>- Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
>- You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

**[题目链接](https://leetcode.com/problems/implement-queue-using-stacks/)**    

**Solution**   
**思路**  
只用一个栈行不行？每次push的时候，直接push，pop和peek要将所有元素pop出。或者pop和peek与stack相同，push时将所有元素pop出来，复杂度太大。  
需要一个容器辅助。这个容器可以是stack，也可以是一个ArrayList，用stack代码量小。  
two stacks:   
- push all elements into stack1;   
- pop elements from stack2, if (stack2 is empty) push elements in stack1 into stack2.  

**代码**   
```java
class MyQueue {
    private Stack<Integer> stack1;
    private Stack<Integer> stack2;
    public MyQueue() {
        stack1 = new Stack();
        stack2 = new Stack();
    }
    public void adjust() {
        if (stack2.empty()) {
            while (!stack1.empty()) {
                stack2.push(stack1.pop());
            }
        }
    }
    // Push element x to the back of queue.
    public void push(int x) {
       stack1.push(x);
     }

    // Removes the element from in front of queue.
    public void pop() {
        adjust();
        stack2.pop();
    }

    // Get the front element.
    public int peek() {
        adjust();
        return stack2.peek();
    }

    // Return whether the queue is empty.
    public boolean empty() {
        adjust();
        return stack2.empty();
    }
}
```
* * *
#### 239. Sliding Window Maximum 

**Description**   
>Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.
>
>For example,
>Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.
```
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
 ```
>Therefore, return the max sliding window as [3,3,5,5,6,7].
>
>Note: 
>You may assume k is always valid, ie: 1 ≤ k ≤ input array's size for non-empty array.
>
>Follow up:
>Could you solve it in linear time?

**[题目链接](https://leetcode.com/problems/sliding-window-maximum/)**    

**Solution**   
**思路**  
思路1，显然本题可以用heap来做，维护一个size为k的max heap，加入元素之前，先将窗口的第一个元素remove，每次取max。    
思路2，第一次做，用双端队列。入队是，先检查队尾元素的值与当前元素值的大小，若当前元素 > deque.peekLast()，从尾部一次poll出，直到队尾>= nums[i]。每次取队头元素。 还要检查队头元素的index是不是窗口第一个元素，若不是，则不用poll除队头，否则poll出队头。   

**代码**   

**heap实现**   
```java
public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return new int[0];
        }
        int[] result = new int[nums.length - k + 1];
        PriorityQueue<Integer> heap = new PriorityQueue<Integer>(10, Collections.reverseOrder());
        for (int i = 0; i < k - 1; i++) {
            heap.offer(nums[i]);
        }
        for (int i = 0; i <= nums.length - k; i++) {
            heap.offer(nums[k - 1 + i]);
            result[i] = heap.peek();
            heap.remove(nums[i]);
        }
        return result;
    }
}
```

**Deque**  
```java
public class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return new int[0];
        }
        int[] result = new int[nums.length - k + 1];
        Deque<Integer> dq = new ArrayDeque<Integer>();
        // descendingly sor the deque, let the elements in deque have descending order
        for (int i = 0; i < nums.length; i++) {
            while (!dq.isEmpty() && nums[i] > nums[dq.peekLast()]) {
               dq.pollLast();
            }
            dq.offer(i);
            if (i >= k - 1) {
                result[i - k + 1] = nums[dq.peek()];
                if (dq.peek() == i - k + 1) {
                    dq.poll();
                }
            }
        }
        return result;
    }
}
```
* * *
#### 341. Flatten Nested List Iterator 

__Description__   
>Given a nested list of integers, implement an iterator to flatten it.
>
>Each element is either an integer, or a list -- whose elements may also be integers or other lists.
>
>Example 1:
>Given the list [[1,1],2,[1,1]],
>
>By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].
>
>Example 2:
>Given the list [1,[4,[6]]],
>
>By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,4,6].


__Solution__  
**思路**   
用stack实现。   
hasNext() : 循环pop出top，提取top的元素，元素反序入栈，知道栈顶isInteger. 
next(): 直接pop除栈顶元素即可。   


```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {
    // similar to binary tree pre-order traverse 
    private Stack<NestedInteger> stack;
    public NestedIterator(List<NestedInteger> nestedList) {
        stack = new Stack(); // 此处出错，没有初始化 @_@......
        if (nestedList != null) {
            for (int i = nestedList.size() - 1; i >= 0; i--) { // i++ 出错， 应为i--, WTF 细心点
                NestedInteger ni = nestedList.get(i);
                if (ni.isInteger() || ni.getList().size() > 0){
                    stack.push(ni);
                }
            }
        }
     }

    @Override
    public Integer next() {
        if (hasNext()) {
            return stack.pop().getInteger();
        }
        return null;
    }

    @Override
    public boolean hasNext() {
        while(!stack.empty() && !stack.peek().isInteger()) {
            NestedInteger ni = stack.pop();
            List<NestedInteger> list = ni.getList();
            for (int i = list.size() - 1; i >= 0; i--) {
                stack.push(list.get(i));
            }
        }
        return !stack.empty();
    }
}

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i = new NestedIterator(nestedList);
 * while (i.hasNext()) v[f()] = i.next();
 */
```
**失败的方法**  
```java
// this solution doesn't work, bacause of nestedInteger like [[[[]][]],[]], stack.size() > 0 but hasNext() == false;
public class NestedIterator implements Iterator<Integer> {
    // similar to binary tree pre-order traverse 
    private Stack<NestedInteger> stack;
    public NestedIterator(List<NestedInteger> nestedList) {
        stack = new Stack(); // 此处出错，没有初始化 @_@......
        if (nestedList != null) {
            for (int i = nestedList.size() - 1; i >= 0; i--) { // i++ 出错， 应为i--, WTF 细心点
                NestedInteger ni = nestedList.get(i);
                if (ni.isInteger() || ni.getList().size() > 0){
                    stack.push(nestedList.get(i));
                }
            }
        }
     }

    @Override
    public Integer next() {
        if (hasNext()) {
            while (!stack.peek().isInteger()) {
                NestedInteger ni = stack.pop();
                List<NestedInteger> l = ni.getList();
                for (int i = l.size() - 1; i >= 0; i--) {
                    stack.push(l.get(i));
                }
            }
            NestedInteger item = stack.pop();
            return item.getInteger();
        }
        return null;
    }

    @Override
    public boolean hasNext() {
        return !stack.empty();
    }
}
 ```
#### 352. Data Stream as Disjoint Intervals 

**Description**   
>Given a data stream input of non-negative integers a1, a2, ..., an, ..., summarize the numbers seen so far as a list of disjoint intervals.
>
>For example, suppose the integers from the data stream are 1, 3, 7, 2, 6, ..., then the summary will be:
```
[1, 1]
[1, 1], [3, 3]
[1, 1], [3, 3], [7, 7]
[1, 3], [7, 7]
[1, 3], [6, 7]
```  
>Follow up:
>What if there are lots of merges and the number of disjoint intervals are small compared to the data stream's size?

**[题目链接](https://leetcode.com/problems/data-stream-as-disjoint-intervals/)**    

**Solution**   
**思路**  
首先interval是排序的，可以用二分法找到目标区间。可以用TreeMap实现这一数据结构。map种存储<start, interval>，start of interval as the key.  
因为可能涉及到合并，需要找到val的上限和下限，分别用treemap的floorKey() and ceilingKey()实现。  
分为4中情况：
1. 做结合
2. 右结合
3. 合并
4. 新建

**代码**   
```java
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
public class SummaryRanges {
    private TreeMap<Integer, Interval> map;
    /** Initialize your data structure here. */
    public SummaryRanges() {
        map = new TreeMap();
    }
    
    public void addNum(int val) {
        Integer f = map.floorKey(val);
        Integer c = map.ceilingKey(val);
        Interval floor = f == null ? null : map.get(f);
        Interval ceil = c == null ? null : map.get(c);
        
        if (floor != null && val <= floor.end + 1) {
            if (ceil != null && val == ceil.start - 1) {
                floor.end = ceil.end;
                map.remove(ceil.start);
            } else {
                floor.end = Math.max(floor.end, val);
            }
        } else if (ceil != null && val == ceil.start - 1) {
            map.put(val, new Interval(val, ceil.end));
            map.remove(ceil.start);
        } else {
            map.put(val, new Interval(val, val));
        }
    }
    
    public List<Interval> getIntervals() {
        return new ArrayList<Interval>(map.values());
    }
}

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges obj = new SummaryRanges();
 * obj.addNum(val);
 * List<Interval> param_2 = obj.getIntervals();
 */
* * *

