146 LRU Cache  
150 Evaluate Reverse Polish Notation  
155 Min Stack  
187 Repeated DNA Sequences  
208 Implement Trie (Prefix Tree)  
211 Add and Search Word – Data structure design  
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
第一次写时，老是报错，找不到原因，后来发现在将word加入到list后直接返回了。。。返回了。。。为啥要返回啊！！！！！！ :scream::scream::scream::scream::scream::scream::scream:

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

