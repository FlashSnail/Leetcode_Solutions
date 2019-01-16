# [Leetcode](https://leetcode.com/) Solutions

This repository is my leetcode solutions, maybe some of these solutions are not best way to accept, but it will be my pleasure if these code can help or inspire you.

I will give some tips in comment.

[TOC]

## 1. Two Sum

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the *same* element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

**CODE:**

```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        ans = set()
        for index, val in enumerate(nums):
            diff = target - val
            if diff in nums and (diff != val or nums.count(diff)>1):
                ans.add(index)
                ans.add(nums.index(diff)) 
        return list(ans)
```



## 2. Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

**CODE:**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        s1 = []
        while(l1):
            s1.append(l1.val)
            l1 = l1.next
        s2 = []
        while(l2):
            s2.append(l2.val)
            l2 = l2.next

        a1=0
        for index, val in enumerate(s1):
            a1+=val*(10**index)
        a2=0
        for index, val in enumerate(s2):
            a2+=val*(10**index)

        #807
        a = str(a1+a2)
        #708
        a = a[::-1]
        node = ListNode(int(a[0]))
        head = node
        for i in a[1:]: 
            child = ListNode(int(i))
            node.next = child
            node = child
        return head
```



## 3. Longest Substring Without Repeating Characters

Given a string, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

**Example 2:**

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**CODE:**

```python
class Solution:
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        ans = []
        res = 0
        for index in range(0,len(s)):
            if s[index] in ans:
                ans = ans[ans.index(s[index])+1:]
            ans.append(s[index])
            res = len(ans) if len(ans)>res else res
        return res
```



## 4. Median of Two Sorted Arrays

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume **nums1** and **nums2** cannot be both empty.

**Example 1:**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

**CODE:**

```python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        i1=i2=0
        ans = []
        for i in range(0, len(nums1)+len(nums2)):
            if i1 == len(nums1):
                ans.extend(nums2[i2:])
                break
            if i2 == len(nums2):
                ans.extend(nums1[i1:])
                break
            if nums1[i1] <= nums2[i2]:
                ans.append(nums1[i1])
                i1+=1
            else:
                ans.append(nums2[i2])
                i2+=1
        if len(ans) % 2 == 0:
            return (ans[len(ans)//2-1]+ans[len(ans)//2])/2
        else:
            return ans[len(ans)//2]
```



## 5. Longest Palindromic Substring

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example 1:**

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: "cbbd"
Output: "bb"
```

**CODE:**

```python
class Solution:
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        #The best way is use dp
        dp=[[False]*(len(s)+1) for i in range(len(s)+1)]
        maxLen = 0
        ans = ''
        for e in range(len(s)):
            b=e
            while(b>=0):
                if s[b]==s[e] and (e-b<2 or dp[e-1][b+1]==True):
                    dp[e][b]= True
                    substr = s[b:e+1]
                    ans = substr if len(substr) >= maxLen else ans
                    maxLen = len(ans)
                b-=1
        return ans
```



## 6. ZigZag Conversion

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string s, int numRows);
```

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**Example 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

**CODE:**

```python
class Solution:
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """
        index = 0
        row = 0
        flag = True
        ans = ''
        zigzag = ['' for i in range(numRows)]
        while(index<len(s)):
            zigzag[row] += s[index]
            if flag:
                row += 1
                if row == numRows:
                    row -= 2
                    flag = False
            else:
                row -= 1
                if row < 0:
                    row += 2
                    flag = True
            index += 1
        for i in range(numRows):
            ans += zigzag[i]
        return ans
```



## 7. Reverse Integer

Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

```
Input: 123
Output: 321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

**Note:**
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^^31^,  2^31^ − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

**CODE:**

```python
class Solution:
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        n = 1
        ans = []
        res = 0
        if x < 0:
            flag = -1
        else :
            flag = 1
        x *= flag
        while(x//n != 0):
            ans.append(x % 10**n // 10**(n-1))
            x -= x%10**n
            n += 1
        for index,i in enumerate(ans):
            res += ans[index]*10**(len(ans)-index-1)
        if res < -2**31 or res > 2**31-1:
            res = 0
        return res*flag        
```



## 8. String to Integer (atoi)

Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

**Note:**

- Only the space character `' '` is considered as whitespace character.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31^,  2^31^ − 1]. If the numerical value is out of the range of representable values, INT_MAX (2^31^ − 1) or INT_MIN (−2^31^) is returned.

**Example 1:**

```
Input: "42"
Output: 42
```

**Example 2:**

```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.
```

**Example 3:**

```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

**Example 4:**

```
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.
```

**Example 5:**

```
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−2**31) is returned.
```

**CODE:**

```python
class Solution:
    def myAtoi(self, str):
        """
        :type str: str
        :rtype: int
        """
        index = 0
        stop = False
        sign = False
        ans = []
        while(index < len(str)):
            if len(ans)!=0:
                stop = True

            if str[index] == ' ':
                index+=1
                if stop:
                    break
                elif not stop and sign:
                    break
                else:
                    continue

            if (str[index] == '-' or str[index] == '+') and not sign:
                if len(ans)!=0:
                    break
                sign = -1 if str[index] == '-' else 1
                index += 1
                continue

            if str[index] >= '0' and str[index] <= '9':
                ans.append(int(str[index]))
                index+=1
            else:
                break

        res = 0
        if sign and not stop and len(ans)!=1:
            return res
        for index, val in enumerate(ans):
            res += val * 10**(len(ans)-index-1)
        sign = 1 if sign == False or sign == 1 else -1 
        res =  res*sign
        if res < -2**31:
            res = -2**31
        if res > 2**31 - 1:
            res = 2**31 - 1
        return res
```



## 9. Palindrome Number

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

**Example 1:**

```
Input: 121
Output: true
```

**Example 2:**

```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

**Follow up:**

Coud you solve it without converting the integer to a string?

**CODE:**

```python
class Solution:
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        #obviously you cannot use int2str api or something
        if x<0:
            return False
        orl = x
        ans = []
        n = 1
        while(x!=0):
            tmp = x % (10)**n
            ans.append(tmp // (10)**(n-1))
            x -= tmp
            n += 1
        res = 0
        for index, val in enumerate(ans):
            res += val * 10**(len(ans)-index-1)
        if res == orl:
            return True
        else:
            return False
```



## 10. Regular Expression Matching

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

**Example 1:**

```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**

```
Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**

```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**

```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
```

**Example 5:**

```
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```

**CODE:**

```python
class Solution:
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        
```

