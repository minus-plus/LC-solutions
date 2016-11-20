# Palindrome 系列
5 Longest Palindromic Substring  
9 Palindrome Number  
125 Valid Palindrome  
131 Palindrome Partitioning  
132 Palindrome Partitioning II  
214 Shortest Palindrome  
336 Palindrome Pairs  

---

#### 5. Longest Palindromic Substring
__Description__  
Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.  
判断字符串的最长回文串。  
**Solution**  
**思路：**  
枚举start和end，判断子串是否是回文串，枚举复杂度O( 2^n)，判断子串是否是回文串双指针法复杂度O(n)。总复杂度O(n^^3)。  
**优化：**  
枚举位置i，复杂度O(n)，向两边延伸，枚举长度，复杂度O(n)。不提取子串，当然也不用判断子串是否是回文串。总复杂度降为O(n^2)。
```java
public class Solution {
    public int getLength(String s, int left, int right) {
        while (left >= 0 && right < s.length()) {
            if (s.charAt(left) == s.charAt(right)) {
                left--;
                right++;
            } else {
                break;
            }
        }
        return right - left - 1;
    }
    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        int len = s.length();
        String sub = s.substring(0, 1);
        int max = 1;
        for (int i = 1; i < len; i++) {
            
            if (s.charAt(i - 1) == s.charAt(i)) {
                int even = getLength(s, i - 1, i);
                if (even > max) {
                    max = even;
                    sub = s.substring(i - even / 2, i + even / 2);
                }
            }
            if (i < len - 1 && s.charAt(i - 1) == s.charAt(i + 1)) {
                int odd = getLength(s, i - 1, i + 1);
                if (odd > max) {
                    max = odd;
                    sub = s.substring(i - odd / 2, i + odd / 2 + 1);
                }
            }
        }
        return sub;
    }
    /*
    public static void main(String[] args) {
        Solution5 s5 = new Solution5();
        String s = "abcabdedbacbajfieqowjfioqw";
        String re = s5.longestPalindrome(s);
        System.out.println(re);
    }
    */
}
```
#### 9. Palindrome Number 
__Description__  
Determine whether an integer is a palindrome. Do this without extra space.   
判断整型数字是否是回文。负数不是回文数字。  
**Solution**  
思路：将数字反序，与原数字比较，看是否相等。
```java
public class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0) {
            return false;
        }
        if (x < 10) {
            return true;
        }
        int num0 = x;
        int num1 = 0;
        while (num0 > 0) {
            num1 = 10 * num1 + num0 % 10;
            num0 = num0 / 10;
        }
        return num1 == x;
        
    }
    /**
    public static void main(String[] args) {
        Solution9 s9 = new Solution9();
        System.out.println(s9.isPalindrome(191));
        System.out.println(s9.isPalindrome(-1));
        System.out.println(s9.isPalindrome(100));
        System.out.println(s9.isPalindrome(99));
        System.out.println(s9.isPalindrome(9));
        System.out.println(s9.isPalindrome(100001));
        
    }
    */
}
```
#### 125. Valid Palindrome 

**Description**   
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.
判断字符串是否为回文，忽略除数字和字母以外的char。  
**Solution**  
**思路:**  
双指针直接从两边遍历。如果最后l >= r, return true;
```java
public class Solution {
    public boolean isAlphanumeric(char c) {
        return Character.isLetter(c) || Character.isDigit(c);
    }
    public char lowerCase(char c) {
        return Character.toLowerCase(c);
    }
    public boolean isPalindrome(String s) {
        if (s == null ) {
            return false;
        }
        int l = 0;
        int r = s.length() - 1;
        while (l < r) {
            if (!isAlphanumeric(s.charAt(l))) {
                l++;
                continue;
            }
            if (!isAlphanumeric(s.charAt(r))) {
                r--;
                continue;
            }
            if (lowerCase(s.charAt(l)) == lowerCase(s.charAt(r))) {
                l++;
                r--;
            } else {
                break;
            }
        }
        return l >= r;
    }
}
```
#### 131. Palindrome Partitioning

**Description**   
Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

For example, given s = "aab",
Return
```
[
  ["aa","b"],
  ["a","a","b"]
]
```  
**Solution**    
**思路**   
划分类或组合类问题，DFS。  
为什么查找所用情况时不能返回boolean值，因为在非叶子节点时，一旦返回boolean值，就会停止搜索。  
而对于查找到第一个目标就停止的搜索，适于返回boolean值。
```java
import java.util.*;
public class Solution {
    public boolean isPalindrome(String s) {
        int start = 0;
        int end = s.length() - 1;
        while (start < end) {
            if (s.charAt(start) != s.charAt(end)) {
                return false;
            }
            start++;
            end--;
        }
        return true;
    }
    public void dfs(String s, List<List<String>> result, List<String> path, int pos) {
        if (pos == s.length()) {
            // leaf state and is goal state
            result.add(new ArrayList<String>(path));
            return;
        }
        for (int i = pos; i < s.length(); i++) {
            String sub = s.substring(pos, i + 1);
            if (isPalindrome(sub)) {
                path.add(sub);
                dfs(s, result, path, i + 1);
                path.remove(path.size() - 1);
            }
        }        
    }
    public List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<List<String>>();
        if (s == null) {
            return result;
        }
        List<String> path = new ArrayList<String>();
        dfs(s, result, path, 0);
        return result;
    }
    /**
    public static void main(String[] args) {
        Solution131 s131 = new Solution131();
        System.out.println(s131.partition(""));
        System.out.println(s131.partition("aaaaa"));
        System.out.println(s131.partition("aaab"));
    }
    */
}
```

#### 132. Palindrome Partitioning II 

**Description**   
Given a string s, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

For example, given s = "aab",
Return 1 since the palindrome partitioning ["aa","b"] could be produced using 1 cut.  
**Solution**  
**思路**    
DFS，TLE。栈层数太高。isPalindrome有重复计算。  
用DP。
所谓重复计算，我的理解是同一个节点或位置被重复访问多次本例中，若用dfs，s.charAt(i) == s.charAt(j)被重复计算多次。
```java
class Solution{
    public int minCut(String s) {

        char[] str = s.toCharArray();
        int len = s.length();
        int[] cut = new int[len];
        boolean[][] dp = new boolean[len][len];
        
        for (int i = 0; i < len; i++) {
            int min = i;
            for (int j = 0; j <= i; j++) {
                if (str[i] == str[j] && (j + 1 > i - 1 || dp[j + 1][i - 1])) {
                    dp[j][i] = true;
                    if (j == 0) {
                        min = 0;
                    } else {
                        min = Math.min(min, cut[j - 1] + 1);
                    }
                }
            }
            cut[i] = min;
        }
        return cut[len - 1];
    }
    /**
    public static void main(String[] args) {
        Solution132DP s132 = new Solution132DP();
        System.out.println(s132.minCut(""));
        System.out.println(s132.minCut("jfewqiojfeqiowjoi"));
        System.out.println(s132.minCut("adaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaedd"));
        System.out.println(s132.minCut("aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"));
       
    }
    */
}
```
#### 214. Shortest Palindrome 
**Description**   
Given a string S, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

For example:

Given "aacecaaa", return "aaacecaaa".

Given "abcd", return "dcbabcd".  
**Solution**   
**思路**  
从start开始或从end开始找最长回文子串，然后在左边或右边镜像添加。**LTE**。要用KMP算法，先跳过。
```java
public class Solution {
    public String shortestPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        int len = s.length();
        StringBuilder sb = new StringBuilder();
        for (int i = len - 1; i >= 0; i--) {
            sb.append(s.charAt(i));
        }
        int i;
        for (i = len - 1; i >= 0; i--) {
            if (s.substring(0, i + 1).equals(sb.substring(len - 1 - i, len))) {
                break;
            }
        }
        return sb.substring(0, len - i - 1) + s;
    }
}
```
#### 336. Palindrome Pairs 

**Description**   
Given a list of unique words. Find all pairs of distinct indices (i, j) in the given list, so that the concatenation of the two words, i.e. words[i] + words[j] is a palindrome.  

Example 1:  
Given words = ["bat", "tab", "cat"]  
Return [[0, 1], [1, 0]]  
The palindromes are ["battab", "tabbat"]  
Example 2:  
Given words = ["abcd", "dcba", "lls", "s", "sssll"]  
Return [[0, 1], [1, 0], [3, 2], [2, 4]]  
The palindromes are ["dcbaabcd", "abcddcba", "slls", "llssssll"]  

**Solution**  
**思路**  
```java

```










