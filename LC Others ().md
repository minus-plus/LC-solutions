#### LinkedList
2 Add Two Numbers  
3 Longest Substring Without Repeating Characters  
4 Median of Two Sorted Arrays  
6 ZigZag Conversion   
7 Reverse Intege  
8 String to Integer (atoi)    
10 Regular Expression Matching 
11 Container With Most Water 
44 Wildcard Matching 
* * * 
#### 2. Add Two Numbers 

**Description**   
>You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
>
>Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
>Output: 7 -> 0 -> 8

**[题目链接](https://leetcode.com/problems/add-two-numbers/)**    

**Solution**   
**思路**  
链表，合并。水果~

**代码**   
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(-1);
        ListNode tail = head;
        int carry = 0;
        while (l1 != null && l2 != null) {
            int num = l1.val + l2.val + carry;
            ListNode node = new ListNode(num % 10);
            tail.next = node;
            tail = node;
            carry = num / 10;
            l1 = l1.next;
            l2 = l2.next;
        }
        while (l1 != null) {
            int num = l1.val + carry;
            ListNode node = new ListNode(num % 10);
            tail.next = node;
            tail = node;
            carry = num / 10;
            l1 = l1.next;
        }
        while (l2 != null) {
            int num = l2.val + carry;
            ListNode node = new ListNode(num % 10);
            tail.next = node;
            tail = node;
            carry = num / 10;
            l2 = l2.next;
        }
        if (carry != 0) {
            tail.next = new ListNode(carry);
        }
        return head.next;
    }
}
```
或这样写 
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(-1);
        ListNode tail = head;
        int num = 0;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int n1 = l1 != null ? l1.val : 0;
            int n2 = l2 != null ? l2.val : 0;
            num  = n1 + n2 + carry;
            tail.next = new ListNode(num % 10);
            carry = num / 10;
            tail = tail.next;
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        if (carry != 0) {
            tail.next = new ListNode(carry);
        }
        return head.next;
    }
}
```
* * *
#### 3. Longest Substring Without Repeating Characters

**Description**   
>Given a string, find the length of the longest substring without repeating characters.
>
>Examples:
>
>Given "abcabcbb", the answer is "abc", which the length is 3.
>
>Given "bbbbb", the answer is "b", with the length of 1.
>
>Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.


**[题目链接](https://leetcode.com/problems/longest-substring-without-repeating-characters/)**    

**Solution**   
**思路**  
HashMap记录元素的index + two pointers扫描。  

**代码**   
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int pointer1 = 0;
        int pointer2 = 0;
        int max = 0;
        HashMap<Character, Integer> hm = new HashMap();
        while (pointer2 < s.length()) {
            char c = s.charAt(pointer2);
            //ignore all elements before pointer1
            // elements before pointer1 is useless even bad  
            // to make sure pointer1 will not go back
            if (hm.containsKey(c) && hm.get(c) >= pointer1) {
                pointer1 = hm.get(c) + 1;
                hm.put(c, pointer2);
            } else {
                hm.put(c, pointer2);
                max = Math.max(max, pointer2 - pointer1 + 1);
            }
            pointer2++;
        }
        return max;
    }
}
```
* * *
#### 4. Median of Two Sorted Arrays

__Description__   
>There are two sorted arrays nums1 and nums2 of size m and n respectively.
>
>Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

>Example 1:
```
nums1 = [1, 3]
nums2 = [2]
The median is 2.0
```
Example 2:
```
nums1 = [1, 2]
nums2 = [3, 4]
The median is (2 + 3)/2 = 2.5
```

**[题目链接](https://leetcode.com/problems/median-of-two-sorted-arrays/)**   
__Solution__  
思路：复杂度O(log(m + n))，考虑二分法。
1. 对nums1和nums2分别二分，状态无法转移，舍弃m / 2或n / 2后，不知道之后找第几大数。此法不通。
2. 对k二分，在nums1和nums2中分别找第k / 2个数，比较两者大小，每次舍弃 k / 2个数，之后找第k - k /  2个数。

```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len = nums1.length + nums2.length;
        if (len % 2 == 1) {
            return findKth(nums1, nums2, 0, 0, len / 2 + 1) * 1.0;
        } else {
            return (findKth(nums1, nums2, 0, 0, len / 2) + findKth(nums1, nums2, 0, 0, len / 2 + 1)) / 2.0; 
        }
    }
    public int findKth(int[] nums1, int[] nums2, int start1, int start2, int k) {
        // illege condition
        if (start1 >= nums1.length) {
            return nums2[start2 + k - 1];
        }
        if (start2 >= nums2.length) {
            return nums1[start1 + k - 1];
        }
        if (k == 1) {
            return Math.min(nums1[start1], nums2[start2]);
        }
        // k / 2, 分赃不均不要紧，舍弃前 k / 2, 剩余k - k / 2;
        int key1 = start1 + k / 2  - 1 < nums1.length ? nums1[start1 + k / 2 - 1] : Integer.MAX_VALUE;
        int key2 = start2 + k / 2  - 1 < nums2.length ? nums2[start2 + k / 2 - 1] : Integer.MAX_VALUE;
        // key1, key2, 谁小丢弃谁，因为小意味着median肯定在k / 2后
        if (key1 < key2) { // discard nums1
            return findKth(nums1, nums2, start1 + k / 2, start2, k - k / 2);
        } else {
            return findKth(nums1, nums2, start1, start2 + k / 2, k - k / 2);
        }
    }
}
```
* * *
#### 6. ZigZag Conversion 

**Description**   
>The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of >rows like this: (you may want to display this pattern in a fixed font for better legibility)
```
P   A   H   N
A P L S I I G
Y   I   R
```
>And then read line by line: "PAHNAPLSIIGYIR"
>Write the code that will take a string and make this conversion given a number of rows:
>
>string convert(string text, int nRows);
convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

**[题目链接](https://leetcode.com/problems/zigzag-conversion/)**    

**Solution**   
**思路**  
```
0             8 
1          7  9
2      6      10
3  5          11
4             12 . . .
```
1. 认真考虑重复周期是什么？A: 2 * numRows - 2。  
2. row i, 应该方位哪些位置呢？A: j % period == i || j % period ==  period - i, first i or last i - 1个。for the last i - i in the perioid, its position should be period - 1(i.e. last pos in perioid) - (i - 1).   


**代码**   
```java
public class Solution {
    public String convert(String s, int numRows) {
        if (numRows <= 1) {
            return s;
        }
        StringBuilder sb = new StringBuilder();
        int period = 2 * numRows - 2;
        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j < s.length(); j++) {
                if (j % period == i || j % period == period - i) {
                    sb.append(s.charAt(j));
                }
            }
        }
        return sb.toString();
    }
}
```
* * *
#### 7. Reverse Intege

**Description**   
>Reverse digits of an integer.
>
>Example1: x = 123, return 321
>Example2: x = -123, return -321
>
>Have you thought about this?
>Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!
>
>If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.
>
>Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?
>
>For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.


**[题目链接](https://leetcode.com/problems/reverse-integer/)**    

**Solution**   
**思路**  
关键是考虑溢出的问题。现将x最为long反转，返回long。然后查看是否溢出。  

**代码**   
```java
public class Solution {
    public long reverseLong(long x) {
        if (x < 0) {
            return -reverseLong(-x);
        }
        long result = 0L;
        while (x != 0) {
            result *= 10;
            result += x % 10;
            x /= 10;
        }
        return result;
    }
    public int reverse(int x) {
        long result = reverseLong(x);
        if (result < Integer.MIN_VALUE || result > Integer.MAX_VALUE) {
           return 0;
        }
        return (int)result;
    }
}
```
* * *

#### 8. String to Integer (atoi) 

**Description**   
>Implement atoi to convert a string to an integer.
>
>Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.
>
>Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.


**[题目链接](https://leetcode.com/problems/string-to-integer-atoi/)**    

**Solution**   
**思路**  
看似简单，其实有一些corner case需要认真考虑。  
1. 什么时候查看是否溢出。A：每次结算result的时候。 
2. sign的问题。A：所有non-digit char不在位置0都是非法。  

**代码**   
```java
public class Solution {
    public int myAtoi(String str) {
        if (str == null || str.length() == 0) {
            return 0;
        }
        str = str.trim();// remove all ' ' on head and tail
        if (str.length() == 0) {
            return 0;
        }
        long result = 0L;
        int sign = 1;
        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            int val = c - 48;
            if (val >= 0 && val <= 9) {
                result = result * 10 + val;
                if (result * sign >= Integer.MAX_VALUE) {
                    return Integer.MAX_VALUE;
                } else if (result * sign <= Integer.MIN_VALUE) {
                    return Integer.MIN_VALUE;
                }
            } else {
                if (i == 0) {
                    if (c == '-') {
                        sign = -1;
                    } else if (c == '+') {
                        sign = 1;
                    } else {
                        // other non-digit char
                        break;
                    }
                } else {
                    //other position
                    return (int)(result * sign);
                }
            }
        }
        return (int)(result * sign);
    }
}
```
* * *
#### 10. Regular Expression Matching 

**Description**   
>Implement regular expression matching with support for '.' and '*'.
```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```

**[题目链接](https://leetcode.com/problems/regular-expression-matching/)**    

**Solution**   
**思路**  
If the next character of p is NOT ‘\*’, then it must match the current character of s. Continue pattern matching with the next character of both s and p.  
If the next character of p is ‘\*’, then we do a brute force exhaustive matching of 0, 1, or more repeats of current character of p… Until we could not match any more characters.    
first character should not be '\*', because it is non-sense.    
```
'\*: c\*abc = abc, cabc, ccabc, cccabc, ccc..cabc
```

**代码**   
```java
public class Solution {
    public boolean isMatch(String s, String p) {
        if (p.length() == 0) {
            return s.length() == 0;
        }
        if (p.length() > 1 && p.charAt(1) == '*') {
            int i = 0;
            // * matches 1, 2, ...
            while (i < s.length() && (p.charAt(0) == '.' || p.charAt(0) == s.charAt(i))) {
                if (isMatch(s.substring(i + 1), p.substring(2))) {
                    return true;
                }
                i++;
            }
            // * matches 0
            return isMatch(s, p.substring(2));
        } else {
            if (s.length() > 0 && (p.charAt(0) == '.' || p.charAt(0) == s.charAt(0))) {
                return isMatch(s.substring(1), p.substring(1));
            }
            return false;
        }
    }
}
```
* * *
#### 11. Container With Most Water 

**Description**   
>Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**[题目链接](https://leetcode.com/problems/container-with-most-water/)**    

**Solution**   
**思路**  
two pointers，container的最大容积取决于两根线中最短的线的长度和两者的距离。 
~~暴力枚举i和j，复杂度O(n^2)。    
但是否所有的情况都可能是max呢？显然不是。以i, j为例，若i~j之间的pair的最低高度小于i和j的最低高度，则i和j之间的所有情况都不可能小于(i, j)。因此枚举(i, j)后，只需要枚举最低高度> (i, j)的配对。  
可用two pointers，one pass scan搞定。~~


**代码**   
```java
public class Solution {
    public int maxArea(int[] height) {
        if (height == null || height.length < 2) {
            return 0;
        } 
        int max = Integer.MIN_VALUE;
        int left = 0;
        int right = height.length - 1;
        
        while (left < right) {
            max = Math.max(max, Math.min(height[left], height[right]) * (right - left));
            if (height [left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return max;
    }
}
```
* * *

#### 44. Wildcard Matching 

**Description**   
>Implement wildcard pattern matching with support for '?' and '*'.
```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).
The function prototype should be:
bool isMatch(const char *s, const char *p)
Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```

**[题目链接](https://leetcode.com/problems/wildcard-matching/)**    

**Solution**   
**思路**  
思路1，dfs。首先trim p，因为p可能含多个连续的'\*'。分p=='\*'和p!='\*'两种情况讨论。TLE。
思路2，动归。

```
dp[i][j] = dp[i - 1][j - 1] && (i, j match), if p[j] != '\*'
dp[i][j] = dp[i][j - 1] || dp[i - 1][j], if p[j] == '\*', other than dp[i][j] = dp[i][j - 1] || dp[i - 1][j - 1], do you know the reason? 
```

**代码**   
DFS。
```java
public class Solution {
    public String trim(String s) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            if (i != 0 && s.charAt(i) == '*' && s.charAt(i - 1) == '*') {
                continue;
            }
            sb.append(s.charAt(i));
        }
        return sb.toString();
    }
    public boolean isMatch(String s, String p) {
        p = trim(p);
        return isMatchHelper(s, p);
    }
    public boolean isMatchHelper(String s, String p) {
        if (p.length() == 0) {
            return s.length() == 0;
        }
        if (s.length() == 0 && (p.length() > 1 || p.charAt(0) != '*')) {
            return false;
        }
        if (p.charAt(0) != '*') {
            if (s.length() > 0 && (p.charAt(0) == '?' || p.charAt(0) == s.charAt(0))) {
                return isMatchHelper(s.substring(1), p.substring(1));
            } else {
                return false;
            }
        } else {
            // * could matches any length of sequece in s
            // p go to last *
            int i = 0;
            while (i < s.length()) {
                if (isMatchHelper(s.substring(i + 1), p.substring(1))) {
                    return true;
                }
                i++;
            }
            // * matches empty
            return isMatchHelper(s, p.substring(1));
        }
    }
}
```
DP
```java
public class Solution {
    public boolean isMatch(String s, String p) {
        boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
        dp[0][0] = true;
        for (int j = 1; j <= p.length(); j++) {
            dp[0][j] = dp[0][j - 1] && p.charAt(j - 1) == '*';
        }
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 1; j <= p.length(); j++) {
                if (p.charAt(j - 1) == '*') {
                    dp[i][j] = dp[i][j - 1] || dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j - 1] && (p.charAt(j - 1) == '?' || p.charAt(j - 1) == s.charAt(i - 1));
                }
            }
        }
        return dp[s.length()][p.length()];
    }
}
```
* * * 





