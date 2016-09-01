43 Multiply Strings  
50 Pow(x, n)  
65 Valid Number  
66 Plus One    
67 Add Binary  
149 Max Points on a Line  
166 Fraction to Recurring Decimal  
168 Excel Sheet Column Title  
171 Excel Sheet Column Number  
172 Factorial Trailing Zeroes  
179 Largest Number  
224 Basic Calculator  
227 Basic Calculator II  
233 Number of Digit One  
258 Add Digits  
273 Integer to English Words   


* * * 
#### 43. Multiply Strings 

**Description**   
>Given two numbers represented as strings, return multiplication of the numbers as a string.

**Solution**  
**思路**  
模拟乘法。两个数相乘结果最多有几位呢？  
123 * 4 = 492;
999 * 9 = 8991;
因此最多有m + n位。
建一个length = m + n的数组，两个数从右到左算，num1[i]和num2[j]相乘，个位在result[i + j + 1]，十位应在result[i + j]。  
因此: 
	`result[i + j] = (result[i + j + 1] + num1[i] * num2[j])  / 10`;  
	`result[i + j + 1] = (result[i + j + 1] + num1[i] * num2[j])  % 10`;

```java
public class Solution {
    public String multiply(String num1, String num2) {
        if (num1 == null || num2 == null) {
            return null;
        }
        int m = num1.length();
        int n = num2.length();
        
        int[] result = new int[m + n];
        for (int i = m - 1; i >= 0; i--) {
            int c1 = num1.charAt(i) - '0';
            for (int j = n - 1; j >= 0; j--) {
                int c2 = num2.charAt(j) - '0';
                int current = c2 * c1 + result[i + j + 1];
                result[i + j] += current / 10;
                result[i + j + 1] = current % 10;
            }
        }
        StringBuffer sb = new StringBuffer();
        int index = 0;
        while (index < m + n && result[index] == 0) {
            index++;
        }
        if (index == m + n) {
            return "0";
        }
        for (int i = index; i < m + n; i++) {
            sb.append(result[i]);
        }
        return sb.toString();
    }
}
```
**改两行代码**  
```java
public class Solution {
    public String multiply(String num1, String num2) {
        if (num1 == null || num2 == null) {
            return null;
        }
        int m = num1.length();
        int n = num2.length();
        
        int[] result = new int[m + n];
        for (int i = m - 1; i >= 0; i--) {
            int c1 = num1.charAt(i) - '0';
            for (int j = n - 1; j >= 0; j--) {
                int c2 = num2.charAt(j) - '0';
                int current = c2 * c1 + result[i + j + 1];
                result[i + j] += current / 10;
                result[i + j + 1] = current % 10;
            }
        }
        StringBuffer sb = new StringBuffer();
        for (int i : result) {
            if (!(sb.length() == 0 && i == 0)) {
                sb.append(i);
            }
        }
        return sb.length() == 0 ? "0" : sb.toString();
    }
}
```
* * *

#### 50. Pow(x, n) 

__Description__   
Implement pow(x, n).
__Solution__  

思路： 
O(n)暴力解法，将底数x乘以自身n - 1次。TLE
考虑log(n)解法。二分。
是否每个x都需要乘一遍呢，显然不是，当进行到一半时，已经得到了后一半相乘得到的数了，因此可以舍弃后一半。
注意要分n为odd和even两种情况考虑。
**代码**  
```java
public class Solution {
    public double getPow(double x, int n) {
        if (n == 0) {
            return 1;
        }
        double half = getPow(x, n / 2);
        half = half * half;
        if (n % 2 == 0) {
            return half;
        } else {
            return half * x;
        }
    }
    public double myPow(double x, int n) {
        double re = getPow(x, Math.abs(n));
        return n < 0 ? 1.0 / re : re;
    }
}
```

```java
public class Solution {
    public double myPow(double x, int n) {
       if (n == 0) {
           return 1;
       }
       if (n == Integer.MIN_VALUE) {
           n++;
           return 1 / x * myPow(x, n);
       }
       if (n < 0) {
           n = -n;
           x = 1.0 / x;
       }
       if (n % 2 == 0) {
           return myPow(x * x, n / 2);
       } else {
           return x * myPow(x * x, n / 2);
       }
    }
}
```
**快速幂解法**  
```java
public class Solution {
    //快速幂求解。注意n < 0的情况。
    public double myPow(double x, int n) {
        double result = 1;
        if (n == Integer.MIN_VALUE) {
            n += 1;
            result *= 1 / x;
        }
        if (n < 0) {
            n = -n;
            x = 1.0 / x;
        }
        
        while (n > 0) {
            if ((n & 1) != 0) {
                result *= x;
            }
            x *= x;
            n = (n >> 1);
        }
        return result;
    }
}
```
* * *

#### 65. Valid Number 

**Description**   
>Validate if a given string is numeric.
>
>Some examples:
`"0"` => `true`
`" 0.1 "` => `true`
`"abc"` => `false`
`"1 a"` => `false`
`"2e10"` => `true`
Note: It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one.


**Solution**  
**思路**   
**逻辑要清楚**  
写代码时，先if else出来各个分支，再对各个分支各个击破，if() else时尽量不要进行复杂的bool运算，没必要，也很容易出错。if(复杂的boolean operation)尽量用 ：
```
if (booelan) {
	if (boolean) {
	 //
	} else {
	 //
	}
}
```

非法情况： 
数字可能包含的元素['0'-'9', 'e', '.', '+', '-']  
if c 是符号位，c位于pos 0或e之后  
if c 是'.'，c是第一个'.'
if c 是'e'，
if c 是数字
if c 是其他

```java
public class Solution {
    public boolean isNumber(String s) {
        if (s == null) {
            return false;
        }
        s = s.trim();
        if (s.length() == 0) {
            return false;
        }
        int numN = 0;
        int numE = 0;
        int numP = 0;
        int indexE = s.length();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == '+' || c == '-') {
                if (i != 0 && s.charAt(i - 1) != 'e' || i == s.length() - 1) {
                    return false;
                }
            } else if (c == 'e') {
                numE++;
                if (numN == 0 || numE > 1 || i == s.length() - 1) {
                    return false;
                }
                indexE = i;
                numN = 0;
            } else if (c == '.') {
                numP++;
                if (i > indexE || numP > 1) {
                    return false;
                }
            } else if (Character.isDigit(c)) {
                numN++;
            } else {
                return false;
            }
        }
        if (numN == 0) {
            return false;
        }
        return true;
    }
}
```
* * *
#### 66. Plus One 

**Description**   
>Given a non-negative number represented as an array of digits, plus one to the number.
>
>The digits are stored such that the most significant digit is at the head of the list.

**Solution**  
**思路**  
注意进位。  
注意计算完第0位以后carry是否为1，若为1则新建一个数组，将第0为置为1即可。
```java
public class Solution {
    public int[] plusOne(int[] digits) {
        if(digits == null || digits.length == 0) {
           return digits; 
        }
        int carry = 1;
        for(int i = digits.length - 1; i >= 0; i--) {
            int digit = (carry + digits[i]) % 10;
            carry = (carry + digits[i]) / 10;
            digits[i] = digit;
        }
        if(carry == 0) {
            return digits;
        }
        // creat new list
        int[] nums = new int[digits.length + 1];
        nums[0] = 1;
        return nums;
    }
}
```
* * *
#### 67. Add Binary 

**Description**   
>Given two binary strings, return their sum (also a binary string).
>
>For example,
>a = "11"
b = "1"
>Return "100".

**Solution**  
**思路**  
注意进位。
```java
public class Solution {
    public String addBinary(String a, String b) {
        // get length, who is longer
        int m = a.length();
        int n = b.length();
        if (m < n) {
            return addBinary(b, a);
        }
        char[] arr = new char[m];
        for (int i = n - 1; i >= 0; i--) {
            arr[i + m - n] = b.charAt(i);
        }
        for (int i = m - n - 1; i >= 0; i--) {
            arr[i] = '0'; // otherwise arr[i] will be 
        }
        int carry = 0;
        for (int i = m - 1; i >= 0; i--) {
            int temp = carry + (arr[i] - '0') + (a.charAt(i) - '0'); // two '0' is 2 * 48
            arr[i] = (char)(temp % 2 + '0');
            carry = temp / 2;
        }
        String result = new String(arr);
        if (carry > 0) {
            return carry + result;
        }
        char c = '1' - '0';
        System.out.println(c);
        return result;
    }
}
``` 

第二种写法   
```java
public class Solution {
    public String addBinary(String a, String b) {
        // get length, who is longer
        String s = "";
        int add = 0;
        int len = Math.max(a.length(), b.length());
        
        for(int i = 0; i < len; i ++){
            int aa = (a.length() > i) ? a.charAt(a.length() - 1 -i) - '0' : 0;
            int bb = (b.length() > i) ? b.charAt(b.length() - 1 -i) - '0' : 0;
            s = (aa + bb + add) % 2 + s;
            add = (aa + bb + add) / 2;
        }
        if(add == 1){
           s = "1" + s;
        }
        return s;
    }
}
```
* * * 
#### 149. Max Points on a Line 

**Description**   
>Given n points on a 2D plane, find the maximum number of points that lie on the same straight line.

**Solution**  
**思路**  
二维遍历O(n^2)。两个点与中心点p的斜率相同则共线。注意多个点可能有同一个坐标，因此需要记录坐标相同的点的个数。
```java
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */
public class Solution {
    public int maxPoints(Point[] points) {
        int m = points.length;
        int result = 0;
        HashMap<Double, Integer> hm = new HashMap<Double, Integer>();
        for (int i = 0; i < m; i++) {
            hm.clear();
            int same = 0;
            int max = 0;
            for (int j = 0; j < m; j++) {
                if (points[i].x == points[j].x && points[i].y == points[j].y) {
                    same++;
                } else if (points[i].x == points[j].x) {
                    if (!hm.containsKey(Double.MAX_VALUE)) {
                        hm.put(Double.MAX_VALUE, 1);
                    } else {
                        hm.put(Double.MAX_VALUE, hm.get(Double.MAX_VALUE) + 1);
                    }
                    max = Math.max(max, hm.get(Double.MAX_VALUE));
                } else {
                    double k = 1.0 * (points[i].y - points[j].y) / (points[i].x - points[j].x);
                    if (!hm.containsKey(k)) {
                        hm.put(k, 1);
                    } else {
                       hm.put(k, hm.get(k) + 1); 
                    }
                    max = Math.max(max, hm.get(k));
                }
            }
            result = Math.max(result, max + same);
        }
        return result;
    }
}
```
**slighly modified**    
```java 
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */
public class Solution {
    public int maxPoints(Point[] points) {
        int m = points.length;
        int result = 0;
        HashSet<String> set = new HashSet<String>();
        HashMap<Double, Integer> hm = new HashMap<Double, Integer>();
        String p = "";
        for (int i = 0; i < m; i++) {
            p = points[i].x + "" + points[i].y;
            if (set.contains(p)) {
                continue;
            }
            set.add(p);
            hm.clear();
            int same = 0;
            int max = 0;
            for (int j = 0; j < m; j++) {
                if (points[i].x == points[j].x && points[i].y == points[j].y) {
                    same++;
                } else if (points[i].x == points[j].x) {
                    if (!hm.containsKey(Double.MAX_VALUE)) {
                        hm.put(Double.MAX_VALUE, 1);
                    } else {
                        hm.put(Double.MAX_VALUE, hm.get(Double.MAX_VALUE) + 1);
                    }
                    max = Math.max(max, hm.get(Double.MAX_VALUE));
                } else {
                    double k = 1.0 * (points[i].y - points[j].y) / (points[i].x - points[j].x);
                    if (!hm.containsKey(k)) {
                        hm.put(k, 1);
                    } else {
                       hm.put(k, hm.get(k) + 1); 
                    }
                    max = Math.max(max, hm.get(k));
                }
            }
            result = Math.max(result, max + same);
        }
        return result;
    }
}
```
* * *

#### 166. Fraction to Recurring Decimal 

**Description**   
>Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.
>
>If the fractional part is repeating, enclose the repeating part in parentheses.
>
>For example,
>
>Given numerator = 1, denominator = 2, return "0.5".
>Given numerator = 2, denominator = 1, return "2".
>Given numerator = 2, denominator = 3, return "0.(6)".
>Hint:
>
>No scary math, just apply elementary math knowledge. Still remember how to perform a long division?
>Try a long division on 4/9, the repeating part is obvious. Now try 4/333. Do you see a pattern?
>Be wary of edge cases! List out as many test cases as you can think of and test your code thoroughly. 

**Solution**  
**思路**  
除一个数试试。4 / 333 = 0.012012...。如何判断从什么时候开始循环呢？本例从小数点后4位开始重新出现了0，因此从此处开始循环。因此需要将所有mod过的结果存起来，就能知道从何处开始出现了当前的重复值。  

几个重要过程
1. 每步相除得到的结果
2. 每步相模得到的结果
3. 何时停止计算
	* 模不为0或模开始重复计算
4. 何时加小数点
	* 当第一次除后，模不为0（仍然接着往下算），添加小数点
5. 如何确定在什么时候开始重复。如何在结果中加括号
	* 每次将模添加到hashmap，key = m, value = sb.length()， 当m第二次出现时，repeat开始的index = hm.get(m)。结束计算。
0. 如何处理溢出问题，当numerator或denominator = - Integer.MIN_VALUE是会出现溢出问题。因此要用long型数据存储numerator和denominator。

```java
public class Solution {
    public String fractionToDecimal(int a, int b) {
        long numerator = a;
        long denominator = b;
        StringBuilder sb = new StringBuilder();
        if (numerator * 1.0 / denominator < 0) {
            sb.append('-');
            if (numerator < 0) {
               numerator = -numerator;
            } else {
                denominator = -denominator;
            }
        }
        HashMap<Long, Integer> hm = new HashMap<Long, Integer>();
        boolean point = true;
        int repeat = -1;
        int count = -1;
        while (true) {
            long m = numerator % denominator;
            long n = numerator / denominator;
            sb.append(n);
            if (m == 0) {
                break;
            }
            if (point) {
                sb.append('.');
                point = false;
            }
            if (hm.containsKey(m)) {
                repeat = hm.get(m);
                break;
            } else {
                hm.put(m, sb.length());
            }
            numerator = m * 10;
        }
        if (repeat > 0) {
            return sb.substring(0, repeat) +  "(" + sb.substring(repeat, sb.length()) + ")";
        }
        return sb.toString();
    }
}
```
* * *
#### 168. Excel Sheet Column Title 

**Description**   
>Given a positive integer, return its corresponding column title as appear in an Excel sheet.
>
>For example:
```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
```

**Solution**  
**思路**  
26一个周期，因此是26进制。不过不从0开始，因此每一轮n需要自减1。  
```java
public class Solution {
   public String convertToTitle(int n) {
        StringBuilder sb = new StringBuilder();
        while(n > 0){
             // because column start from 1 other than 0 
            char ch = (char) ((n - 1) % 26 + 'A');
            n = (n - 1) / 26;
            sb.append(ch);
        }
        sb.reverse();
        return sb.toString();
    }
}
```
* * *

#### 171. Excel Sheet Column Number 
**Description**   
>Related to question Excel Sheet Column Title
>
>Given a column title as appear in an Excel sheet, return its corresponding column number.
>
>For example:
```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
```

**Solution**  
**思路**  
与上一题相反。本题是将26进制转化为10进制。  
```java
import java.lang.Math;
public class Solution {
    public int titleToNumber(String s) {
        int num = 0;
        int len = s.length();
        for (int i = 0; i < len; i++) {
            num += (s.charAt(i) - 'A' + 1) * Math.pow(26, len- 1 -i);
        }
        return num;
    }
}
```
* * *

#### 172. Factorial Trailing Zeroes 

**Description**   
Given an integer n, return the number of trailing zeroes in n!.
Note: Your solution should be in logarithmic time complexity.  

**Solution**  
**思路**  
结尾的0只跟2 * 5有关，有多少个5就对应多少个0。因此本题是要统计1~n中总共有多少个因子5。5 : 1, 10 : 1, 15 : 1, 20 : 1, 25 : 5 .... 。
This question is pretty straightforward.    
Because all trailing 0 is from factors 5 * 2.  
But sometimes one number may have several 5 factors, for example, 25 have two 5 factors, 125 have three 5 factors. In the n! operation, factors 2 is always ample. So we just count how many 5 factors in all number from 1 to n.  

```java
public class Solution {
    public int trailingZeroes(int n) {
    	if (n <= 0)
    		return 0;
        int result = 0;
        while (n >= 5) {
            result += n / 5;
            n /= 5;
        }
    	return result;
    }
}
```

* * *

#### 179. Largest Number 

**Description**   
>Given a list of non negative integers, arrange them such that they form the largest number.
>
>For example, given [3, 30, 34, 5, 9], the largest formed number is 9534330.
>
>Note: The result may be very large, so you need to return a string instead of an integer.


**Solution**  
**思路**  
本题的主题思想是对数组排序。要自己实现comparator。根据题目要求，12 > 121，因为 12.concat(121) > 121.concat(12).
```java
class NumbersComparator implements Comparator<String> { 
    // 前边没有type checker， 后边有，规定比较的类型，一个都不能写错
	@Override
	public int compare(String s1, String s2) {
		return (s2 + s1).compareTo(s1 + s2);
	}
}
public class Solution {
    public String largestNumber(int[] nums) {
        String[] strs = new String[nums.length];
        for (int i = 0; i < nums.length; i++) {
            strs[i] = Integer.toString(nums[i]);
        }
        Arrays.sort(strs, new NumbersComparator());
        if (Integer.parseInt(strs[0]) == 0) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < strs.length; i++) {
            sb.append(strs[i]);
        }
        return sb.toString();
    }
}
```
下边代码不work。因为comparator不支持比较primitive types。
```java

public class Solution {
    class IntegerComparator implements Comparator<Integer> { 
    // 前边没有type checker，后边有，规定比较的类型，一个都不能写错
    	@Override
        public int compare(Integer n1, Integer n2) {
    	   return n2.compareTo(n1);
    	}
    }
    public String largestNumber(int[] nums) {
        Arrays.sort(nums, new IntegerComparator());
        if (nums[0] == 0) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < nums.length; i++) {
            sb.append(nums[i]);
        }
        return sb.toString();
    }
}
```
**附： quick sort**
```
/快速排序  
void quick_sort(int s[], int l, int r)  
{  
    if (l < r)  
    {  
        //Swap(s[l], s[(l + r) / 2]); //将中间的这个数和第一个数交换 参见注1  
        int i = l, j = r, x = s[l];  
        while (i < j)  
        {  
            while(i < j && s[j] >= x) // 从右向左找第一个小于x的数  
                j--;    
            if(i < j)   
                s[i++] = s[j];  
              
            while(i < j && s[i] < x) // 从左向右找第一个大于等于x的数  
                i++;    
            if(i < j)   
                s[j--] = s[i];  
        }  
        s[i] = x;  
        quick_sort(s, l, i - 1); // 递归调用   
        quick_sort(s, i + 1, r);  
    }  
}  
```
* * *

#### 224. Basic Calculator 

**Description**   
>Implement a basic calculator to evaluate a simple expression string.
>
>The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .
>
>You may assume that the given expression is always valid.
>
>Some examples:
```
"1 + 1" = 2
" 2-1 + 2 " = 3
"(1+(4+5+2)-3)+(6+8)" = 23
```

**Solution**  
**思路**  
没有括号的话，只需要记录数字前方的sign就可以了。但括号可以改变括号内部的符号，因此符号内部的符号取决于括号前的的符号（全部符号）* 符号本身。因此要用stack存储符号。

**代码**  
```java
public class Solution {
    public int calculate(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        s = s.trim();
        if (s.length() == 0) {
            return 0;
        }
        Stack<Integer> stack = new Stack<Integer>();//store global sign
        stack.push(1);
        int result = 0;
        int num = 0;
        int sign = stack.peek();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                continue;
            }
            // recorde number
            if (Character.isDigit(c)) {
                num = 10 * num + (c - '0');
            } else {
                result += num * sign;
                num = 0;
                if (c == '+') {
                    sign = stack.peek();
                } else if (c == '-') {
                    sign = -stack.peek();
                } else if (c == '(') {
                    stack.push(sign);
                } else if (c == ')') {
                    stack.pop();
                    sign = stack.peek();
                }
            }
        }
        return result + sign * num;
    }
}
```
* * *

#### 227. Basic Calculator II 

**Description**   
>Implement a basic calculator to evaluate a simple expression string.
>The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.
>
>You may assume that the given expression is always valid.

>Some examples:
```
"3+2*2" = 7
" 3/2 " = 1
" 3+5 / 2 " = 5
```


**Solution**  
**思路**  

always tree got num as temp  
if previous sign is +, temp = num  
if previous sign is '-', temp = -num  
if previous sign is '\*', temp = temp / num  
if previous sign is '/', temp = temp / num  

if previous sign is '+' or '-', we have left , right + or - num, we could merge left and right   
if previous sign is '\*' or '/', we have left, right '\*' or '/' num, we should merge right and num   
```java
import java.util.*;
public class Solution {
    public int calculate(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        s = s.trim();
        int left = 0;
        int right = 0;
        int num = 0;
        char sign = '+';
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                continue;
            }
            if (Character.isDigit(c)) {
                num = num * 10 + (c - '0');
            } 
            if (!Character.isDigit(c) || i == s.length() - 1) {
                if (sign == '+') {
                    left += right;
                    right = num;
                } else if (sign == '-') {
                    left += right;
                    right = -num;
                } else if (sign == '*') {
                    right *= num;
                } else {
                    right /= num;
                }
                num = 0;
                sign = c;
                
            }
        }
        return left + right;
    }
}
```
之前的那一版太复杂了   
```java
import java.util.*;
public class Solution {
    public int calculate(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        Stack<String> stack = new Stack<String>();
        String str = s.trim();
        int len = str.length();
        int start = 0;
        int end = 0;
        while (start < len && end < len) {
            while (end < len && Character.isDigit(str.charAt(end))) {
                end++;
            }
            // deal with number

            if (start < end) {
                String sub = str.substring(start, end);
                if (!stack.empty()) {
                    String top = stack.peek();
                    // check if top of stack is * or /
                    if (top.equals("*") || top.equals("/")) {
                        if (stack.size() >= 2) {
                            stack.pop();
                            int n1 = Integer.parseInt(stack.pop());
                            int n2 = Integer.parseInt(sub);
                            //System.out.println(n1 + " " + n2);
                            if (top.equals("*")) {
                                stack.push(Integer.toString(n1 * n2));
                            } else {
                                stack.push(Integer.toString(n1 / n2));
                            }
                        } else {
                            return 0; // first is * or /
                        }
                    } else {
                        stack.push(sub);
                    }
                } else {
                    stack.push(sub);
                }
            }
        
            while (end < len && !Character.isDigit(str.charAt(end))) {
                char c = str.charAt(end);
                if (c != ' ') {
                    stack.push(Character.toString(c));
                }
                end++;
                start = end;
            }
        }
        int temp = 0;
        int result = 0;
        String top = "";
        while (!stack.empty()) {
            //System.out.println(stack.pop());
            top = stack.pop();
            if (top.equals("+")) {
                result += temp;
            } else if (top.equals("-")) {
                result -= temp;
            } else {
                temp = Integer.parseInt(top);
            }
        }
        /*
        if (!top.equals("-") && !top.equals("+")) {
            result += temp;
        }
        */
        result += temp;
        return result;
    }
   
}
```

#### 233. Number of Digit One 

**Description**   
>Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.
>
>For example:
>Given n = 13,
>Return 6, because digit 1 occurred in the following numbers: 1, 10, 11, 12, 13.
>
>Hint:
>
>Beware of overflow.

**Solution**  
**思路**  
枚举各个位置上出现1的次数。  
举例，133。个位为1时，左边部分可以是00 ~ 13共14个。十位为1时，左边可以是0 ~ 1，是0~9，共20个。百位为1时，3 * 10 + 4 = 34个，因此共68.
将num分为三个部分，left, digit and right。  
```
right = n % base;
digit = (n / base) % 10;
if digit == 0, left = (n / base - 1) / 10 else left = n / base / 10
if digit == 1, result += left * base + right + 1;
if digit > 1, result += left * base + base;
```
**代码**   
```java
public class Solution {
    public int countDigitOne(int n) {
        int left;
        int digit;
        int right;
        int base = 1;
        int result = 0;
        while (base <= n) {
            digit = (n / base) % 10;
            right = n % base;
            // preparaton, let digit > 0 
            if (digit == 0) {
                digit = 9;
                left = (n / base  - 1) / 10;
            } else {
                left = (n / base) / 10;
            }
            if (digit > 0) {
                /*
                if (digit == 1) {
                    result += left * base + right + 1;
                } else { // digit == 2 ~ 9
                    result += (left + 1) * base;
                */
                result += left * base + (digit > 1 ? base : right + 1);
            }
            if ((long) base * 10L > (long)n) {
                break;
            }
            base *= 10;
        }
        return result;
        
    }
}
```
* * * 

#### 258. Add Digits 

**Description**   
>Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.
>
>For example:
>
>Given num = 38, the process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return it.
>
>Follow up:
>Could you do it without any loop/recursion in O(1) runtime  

**Solution**  
**思路**  
直观解法，得到数digit，相加。循环直到num <= 9为止。复杂度O(n)。 
O(1)的话，必然需要从规律中推导出公式来。   
1 ~ 10 ~ 99 :  1 ~ 9， 1~9, 1 ~ 9, 1 ~ 9....，因此1 ~ 9以周期为9的长度出现，因此结果是n % 9，单n % 9时，结果是9而不是0。

**代码**   
```java
public class Solution {
    public int addDigits(int num) {
        int sum;
        while (num > 9) {
            sum = 0;
            while (num > 0) {
                sum += (num % 10);
                num  = num / 10;
            }
            num = sum;
        }
        return num;
    }
}
```

**公式法**  
```java
public class Solution {
    public int addDigits(int num) {
        if(num == 0) {
            return 0;
        }
        else {
            return (num%9 == 0)? 9: num%9;
        }
    }
}
```

* * *

#### 273. Integer to English Words

**Description**   
>Convert a non-negative integer to its english words representation. Given input is guaranteed to be less than 231 - 1.
>
>For example,  
```
123 -> "One Hundred Twenty Three"
12345 -> "Twelve Thousand Three Hundred Forty Five"
1234567 -> "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
```
>Hint:
>
>Did you see a pattern in dividing the number into chunk of words? For example, 123 and 123000.
>Group the number by thousands (3 digits). You can write a helper function that takes a number less than 1000 and convert just that chunk to words.
>There are many edge cases. What are some good test cases? Does your code work with input such as 0? Or 1000010? (middle chunk is zero and should not be printed out)

**Solution**  
**思路**  
group num by 1000, and write a helper function to take care each 3 digits. you should watch out the spaces.  It is tricky.   
可已在循环时在开头和最后一个多一个空格，最后trim掉。  
**代码**   
```java
public class Solution {
    String[] nums1 = new String[]{"Zero", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine"
            ,"Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen","Nineteen"};
    String[] nums2 = new String[] {"", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
    String[] nums3 = new String[] {"Billion", "Million", "Thousand", ""};
    public String convert(int num) {
        StringBuilder sb = new StringBuilder();
        if (num == 0) {
            return sb.toString();
        }
        if (num >= 100) {
            sb.append(nums1[num / 100]);
            sb.append(" ");
            sb.append("Hundred");
            num %= 100;
        }
        if (num >= 20) {
            sb.append(" " + nums2[num / 10]);
            if (num % 10 != 0) {
                sb.append(" " + nums1[num % 10]);
            }
        } else {
            if (num > 0) {
                sb.append(" " + nums1[num]);
            }
        }
        return sb.toString().trim();
        
    }
    public String numberToWords(int num) {
        if (num == 0) {
            return "Zero";
        }
        int base = 1000000000;
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 4; i++) {
            if (num / base > 0) {
                sb.append(convert(num / base));
                sb.append(" ");
                sb.append(nums3[i]);
                sb.append(" ");
            }
            num = num % base;
            base /= 1000;
        }
        return sb.toString().trim();
    }
}
```
* * *
