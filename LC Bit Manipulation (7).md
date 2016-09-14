136 Single Number  
137 Single Number II  
190 Reverse Bits  
191 Number of 1 Bits  
201 Bitwise AND of Numbers Range  
260 Single Number III  
268 Missing Number  
* * * 
#### 136. Single Number 

**Description**   
>Given an array of integers, every element appears twice except for one. Find that single one.

**[题目链接](https://leetcode.com/problems/single-number/)**    

**Solution**   
**思路**  
异或。 
n^n = 0  
n^0 = n

**代码**   
```java
public class Solution {
    public int singleNumber(int[] nums) {
       int re = 0;
       
       for(int i = 0; i < nums.length; i ++){
           re = re ^ nums[i];
       }
       
       return re;
    }
}
```
* * *
#### 137. Single Number II 

**Description**   
>Given an array of integers, every element appears three times except for one. Find that single one.

**[题目链接](https://leetcode.com/problems/single-number-ii/)**      

**Solution**   
**思路**  
How we present a number? For integers that has 32 bits, we could calculate the value of the number from 1s in that number. For example, 01000001010100010. So all we need is get the positions of 1s in that number.  
For this question, we could establish an array with length = 32 to store number of 1s at corresponding position. 
维护一个bit计数器,i.e. int[ ]型数组，当n的第i个bit位为1时，计数器的相应位置+1。
遍历一遍后，if single number ith bit == 0, 计数器的i位置应为3的倍数，else应为3 k + 1。
此方法同样适用于当所有数重复出现k次，除了一个数出现少于l次(l < k)的情况。仅仅将计数器bits[i]  == bit[i] % k。  

**代码**   
```java
public class Solution {
    public int duplicatedNumber(int[] nums, int n, int k) {
         // 初始化计数器
        int[] bits = new int[32];
        // 更新计数器
        for (int num : nums) {
            for (int i = 0; i < 32; i++) {
                if ((num & (1 << i)) != 0) {
                    bits[i] += 1;
                }
            }
        }
        int re = 0;
        // 计算结果
        for (int i = 0; i < 32; i++) {
            int bit = bits[i] % n;
            re |= (bit << i);
        }
        return re / k;
    }
    public int singleNumber(int[] nums) {
       return duplicatedNumber(nums, 3, 1);
    }
}
```
更直接的位运算参考http://www.acmerblog.com/leetcode-single-number-ii-5394.html
* * * 
#### 190. Reverse Bits 

**Description**   
>Reverse bits of a given 32 bits unsigned integer.
>
>For example, given input 43261596 (represented in binary as 00000010100101000001111010011100), return 964176192 (represented in binary as 00111001011110000010100101000000).
>
>Follow up:
>If this function is called many times, how would you optimize it?

**[题目链接](https://leetcode.com/problems/reverse-bits/)**    

**Solution**   
**思路**  
直接做。

**代码**   
```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int result = 0;
        for (int i = 0; i < 32; i++) {
            result = result << 1 | (n & 1);
            n >>>= 1;
        }
        return result;
    }
}
```
* * *
#### 191. Number of 1 Bits 

**Description**   
>Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).
>
>For example, the 32-bit integer ’11' has binary representation 00000000000000000000000000001011, so the function should return 3.

**[题目链接](https://leetcode.com/problems/number-of-1-bits/)**    

**Solution**   
**思路**  
直接做。

方法二，参考别人的。  
先考虑一下我们从一个数减去1的时候发生了什么。  
如： 0111 0000 – 1 = 0110 1111  
就是从最低位到最低的1那一位全部取反。  
接下来，我们看看n & (n-1)  
0111 0000 & 0110 1111 =  0110 0000  
我们看到，最后一个1变成了0，所以我们只需要O(m)次 ，m为1的个数就可以求出1的个数

**代码**   
```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        for (int i = 0; i < 32; i++) {
            count += (n & 1);
            n >>>= 1;
        }
        return count;
    }
}
```

**方法2**  
```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        while (n != 0) {
            n = n & (n - 1);
            count++;
        }
        return count;
    }
}
```
* * *
#### 201. Bitwise AND of Numbers Range 

**Description**   
>Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.
>
>For example, given the range [5, 7], you should return 4.

**[题目链接](https://leetcode.com/problems/bitwise-and-of-numbers-range/)**    

**Solution**   
**思路**  
求最长公共前缀，类似于TCP/IP的子网掩码。做法是每次将m和n同时右移一位并计数，直到m == n为止。 return m << count.  

**代码**   
```java
public class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        int count = 0;
        while (m != n) {
            m >>= 1;
            n >>= 1;
            count++;
        }
        return n << count;
    }
}
```
或

```java
public class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        //求最长公共前缀，类似于TCP/IP的子网掩码，
        int r = Integer.MAX_VALUE;
        while ((m & r) != (n & r)) {
            r =  r << 1;
        }
        return n & r;
    }
}
```
* * *

#### 260. Single Number III 

**Description**   
>Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

>For example:

>Given nums = [1, 2, 1, 3, 2, 5], return [3, 5].

>Note:
>The order of the result is not important. So in the above example, [5, 3] is also correct.
>Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?


**[题目链接](https://leetcode.com/problems/single-number-iii/)**    

**Solution**   
**思路**  
首先想到用XOR，这样得到a^b。那么如何从a^b得到a和b呢？因为a != b，则某一位a合b不相同，设为i。用1 << i可以将数组分为部分，一部分含a，另一部分含b，二次xor扫描，就能将a和b分开。

**代码**   
```java
public class Solution {
    public int[] singleNumber(int[] nums) {
        int xor = 0;
        for (int n : nums) {
            xor ^= n;
        }
        int filter = 0;
        for (int i = 0; i < 32; i++) {
            filter = 1 << i;
            if ((xor & filter) != 0) {
                break;
            }
        }
        
        int a = xor;
        int b = xor;
        for (int n : nums) {
            if ((filter & n) == 0) {
                a ^= n;
            } else {
                b ^= n;
            }
        }
        return new int[]{a, b};
    }
}
```
* * *
#### 268. Missing Number 

**Description**   
>Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.
>
>For example,
>Given nums = [0, 1, 3] return 2.
>
>Note:
>Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

**[题目链接](https://leetcode.com/problems/missing-number/)**    

**Solution**   
**思路**  
xor。  
two pass xor.第一遍扫描0 ~ n，第二遍扫描nums，return最后的结果。  

**代码**   
```java
public class Solution {
    public int missingNumber(int[] nums) {
        // the largest number should be nums.length 
        // first calculate sum
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int x = nums.length;
        for (int i = 0; i < nums.length; i++) {
            x ^= i;
            x ^= nums[i];
        }
        return x;
    }
}
```
* * *


