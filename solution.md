# [Leetcode](https://leetcode.com/) Solutions

This repository is my leetcode solutions, maybe some of these solutions are not best way to accept, but it will be my pleasure if these code can help or inspire you.

I will give some tips in comment.

If you cannot see the table of content, you can download this file and open it use your local markdown editor. Otherwise it's not convenient for you to index problem which you are interested.

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
    def isMatch(self, text, pattern):
        if not pattern:
            return not text

        first_match = bool(text) and pattern[0] in {text[0], '.'}

        if len(pattern) >= 2 and pattern[1] == '*':
            return (self.isMatch(text, pattern[2:]) or
                    first_match and self.isMatch(text[1:], pattern))
        else:
            return first_match and self.isMatch(text[1:], pattern[1:])
        
```



## 11. Container With Most Water

Given *n* non-negative integers *a1*, *a2*, ..., *an* , where each represents a point at coordinate (*i*, *ai*). *n* vertical lines are drawn such that the two endpoints of line *i*is at (*i*, *ai*) and (*i*, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and *n* is at least 2.

 

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

**Example:**

```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

**CODE**

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int right = height.size() - 1;
        int left = 0;
        int ans = 0;
        while (left < right){
            ans = max(ans, min(height[left], height[right])*(right - left));
            if (height[left] < height[right]) {
                left++;
            }
            else{
                right--;
            }
        }
        return ans;
    }
};
```



## 12. Integer to Roman

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```
Input: 3
Output: "III"
```

**Example 2:**

```
Input: 4
Output: "IV"
```

**Example 3:**

```
Input: 9
Output: "IX"
```

**Example 4:**

```
Input: 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```

**Example 5:**

```
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

**CODE**

```C++
class Solution {
public:
    string intToRoman(int num) {
        vector<int> ans;
        vector<char> symbol = {'M', 'D', 'C', 'L', 'X', 'V', 'I'};
        vector<int> value = {1000, 500, 100, 50, 10, 5, 1};
        for(int i=0; i<symbol.size(); i++){
            int v = num / value[i];
            ans.push_back(v);
            num -= (v * value[i]);
        }
        string res = "";
        int j = 0;
        while(j < ans.size()){
            if(ans[j+1] == 4){
                string tmp(ans[j+1], symbol[j]);
                res += symbol[j+1];
                res += symbol[j-ans[j]]; //如果当前位置有值则取前一位(如9)，否则取当前位(如4)
                j += 2;
            }
            else{
                string tmp(ans[j], symbol[j]);
                res += tmp;
                j++;
            }
        }
        return res;
    }
};
```



## 13. Roman to Integer

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```
Input: "III"
Output: 3
```

**Example 2:**

```
Input: "IV"
Output: 4
```

**Example 3:**

```
Input: "IX"
Output: 9
```

**Example 4:**

```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 5:**

```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

**CODE**

```c++
class Solution {
public:
    int romanToInt(string s) {
        map<char,int> m;
        vector<char> symbol = {'M', 'D', 'C', 'L', 'X', 'V', 'I'};
        vector<int> value = {1000, 500, 100, 50, 10, 5, 1};
        for(int i=0; i<symbol.size(); i++){
            m[symbol[i]] = value[i];
        }
        
        if(s.size() == 1){
            return m[s[0]];
        }
        
        int ans = 0;
        int i = 0;
        while(i < s.size() - 1){
            if(m[s[i+1]] > m[s[i]]){
                ans += m[s[i+1]] - m[s[i]];
                i += 2;
            }
            else{
                ans += m[s[i]];
                i++;
            }
        }
        if(m[s[s.size()-1]] <= m[s[s.size()-2]]){
            ans += m[s[s.size()-1]];
        }
        return ans;
    }
};
```



## 14. Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Note:**

All given inputs are in lowercase letters `a-z`.

**CODE**

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.size() == 0){
            return "";
        }
        int len = 4096;
        for(int i = 0; i<strs.size(); i++){
            len = min(len, strs[i].size());
        }
        string ans = "";
        for(int i = 0; i<len; i++){
            char tmp = strs[0][i]
            for(int j=1; j<strs.size(); j++){
                if(strs[j][i] != tmp){
                    return ans;
                }
            }
            ans += tmp;
        }
        return ans;
    }
};
```



## 15. 3Sum

Given an array `nums` of *n* integers, are there elements *a*, *b*, *c* in `nums` such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

**CODE**

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        
        int n_size = nums.size();
        
        // actually the pivot
        for (int nidx=0; nidx<n_size; nidx++) {
            if (nidx > 0 && nums[nidx] == nums[nidx - 1]) {
                continue;
            }
            int l = nidx + 1;
            int r = n_size - 1;
            int rem = -nums[nidx];
            while (l < r) {
                while (nums[l] + nums[r] < rem && l < r) {
                    l++;
                }
                while (nums[l] + nums[r] > rem && l < r) {
                    r--;
                }
                if (l < r && nums[l] + nums[r] == rem) {
                    int numl = nums[l], numr = nums[r];
                    res.push_back(vector<int> {nums[nidx], numl, numr});
                    l++, r--;
                    while (l < n_size && nums[l] == numl) l++;
                    while (r > -1 && nums[r] == numr) r--;
                }
            }
        }
        return res;
    }
};
```



## 16. 3Sum Closest

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example:**

```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

**CODE**

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int diff = INT_MAX, res = 0;
        for(int i=0; i < nums.size()-2; i++){
            int low = i + 1, high = nums.size() - 1;
            while(low < high){
                int sum = nums[i] + nums[low] + nums[high];
                if (sum == target)
                    return sum;
                if (abs(sum - target) < diff){
                    diff = abs(sum - target);
                    res = sum;
                }
                (sum < target)? low++ : high--;
            }
        }
        return res;
    }
};
```



## 17. Letter Combinations of a Phone Number

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**Note:**

Although the above answer is in lexicographical order, your answer could be in any order you want.

**CODE**

```c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> ans;
        if (digits.size()==0){
            return ans;
        }
        map<char,vector<string>> m;
        vector<string> b {"a","b","c"}; m['2'] = b;
        vector<string> c {"d","e","f"}; m['3'] = c;
        vector<string> d {"g","h","i"}; m['4'] = d;
        vector<string> e {"j","k","l"}; m['5'] = e;
        vector<string> f {"m","n","o"}; m['6'] = f;
        vector<string> g {"p","q","r","s"}; m['7'] = g;
        vector<string> h {"t","u","v"}; m['8'] = h;
        vector<string> i {"w","x","y","z"}; m['9'] = i;
        if (digits.size()==1){
            return m[digits[0]];
        }
        vector<string> tmp = m[digits[0]];
        for(int i = 1; i < digits.size(); i++){
            for(int j = 0; j < tmp.size(); j++){
                for(int k = 0; k < m[digits[i]].size(); k++){
                    ans.push_back(tmp[j] + m[digits[i]][k]);
                }
            }
            vector<string> tmp = ans;
        }
        return ans;
    }
};
```



## 18. 4Sum

Given an array `nums` of *n* integers and an integer `target`, are there elements *a*, *b*, *c*, and *d* in `nums` such that *a* + *b* + *c* + *d* = `target`? Find all unique quadruplets in the array which gives the sum of `target`.

**Note:**

The solution set must not contain duplicate quadruplets.

**Example:**

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

**CODE**

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        int n = nums.size();
        vector<vector<int>> output;
        if(n < 4) return output;
        sort(nums.begin(), nums.end());
        for(int i=0; i<n-3; i++){
            if(i > 0 and nums[i-1] == nums[i]) continue;
            for(int j=i+1; j<n-2; j++){
                if(j > i+1 and nums[j-1] == nums[j]) continue;
                int left = j + 1, right = n - 1;
                while(left < right){
                    int sum = nums[i] + nums[j] + nums[left] + nums[right];
                    if(sum < target) left++;
                    else if(sum > target) right--;
                    else{
                        output.push_back({nums[i], nums[j], nums[left++], nums[right--]});
                        while(left < right and nums[left] == nums[left-1]) left++;
                        while(left < right and nums[right] == nums[right+1]) right--;
                    }
                }
            }
        }
        return output;
    }
};
```



## 19. Remove Nth Node From End of List

Given a linked list, remove the *n*-th node from the end of list and return its head.

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given *n* will always be valid.

**Follow up:**

Could you do this in one pass?

**CODE**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        vector<ListNode*> arr;
        ListNode* node = head;
        ListNode* ans;
        int len = 0;
        while(node != NULL){
            arr.push_back(node);
            node = node->next;
            len++;
        }
        if(len-n-1 >= 0){
            arr[len-n-1]->next = arr[len-n]->next;
            return head;
        }
        else if((len-n == 0) && (len > 1)){
            return arr[len-n+1];
        }
        else{
            return NULL;
        }
    }
};
```



## 20. Valid Parentheses

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "()[]{}"
Output: true
```

**Example 3:**

```
Input: "(]"
Output: false
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true
```

**CODE**

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;
        int len = s.size();
        if(len % 2 != 0)
            return false;
        for(int i=0; i<len; i++){
            if((s[i] == '(') || (s[i] == '[') || (s[i] == '{'))
                stk.push(s[i]);
            else{
                //cout << stk.top();
                if(!stk.empty() && s[i] == ')' && stk.top() == '(')
                    stk.pop();
                else if(!stk.empty() && s[i] == ']' && stk.top() == '[')
                    stk.pop();
                else if(!stk.empty() && s[i] == '}' && stk.top() == '{')
                    stk.pop();
                else
                    return false;
            }
        }
        if(stk.empty())
            return true;
        else
            return false;
    }
};
```



## 21. Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

**CODE**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        // be careful, you must new a class object!
        ListNode* ans = new ListNode(0);
        ListNode* tmp = new ListNode(0);
        ListNode* head = new ListNode(0);
        if(l1 == NULL && l2 != NULL)
            return l2;
        else if (l1 != NULL && l2 == NULL)
            return l1;
        else if (l1 == NULL && l2 == NULL)
            return NULL;
        else{
            if (l1->val <= l2->val){
                head->val = l1->val;
            	l1 = l1->next;
            }
            else{
                head->val = l2->val;
            	l2 = l2->next;
            }
        }
        ans = head;
        while(l1 != NULL && l2 != NULL){
            if (l1->val <= l2->val){
                tmp = l1;
                l1 = l1->next;
            }
            else{
                tmp = l2;
                l2 = l2->next;
            }
            ans->next = tmp;
            ans = ans->next;
        }
        while(l1 != NULL){
            ans->next = l1;
            ans = ans->next;
            l1 = l1->next;
        }
        while(l2 != NULL){
            ans->next = l2;
            ans = ans->next;
            l2 = l2->next;
        }
        return head;
    }
};
```



## 22. Generate Parentheses

Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given *n* = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

**CODE**

```c++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        func(ans, "", 0, 0, n);
        return ans;
    }
    void func(vector<string>& ans, string cur,int left, int right, int n) {
        if(cur.size() == n*2){
            ans.push_back(cur);
            return;
        }
        if(left < n){
            func(ans, cur+"(", left+1, right, n);
        }
        if(right < left){
            func(ans, cur+")", left, right+1, n);
        }
    }
};
```



## 23. Merge k Sorted Lists

Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Example:**

```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

**CODE**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* head = new ListNode(0);
        ListNode* cur = head;
        int index = 0;
        while(true){
            bool flag = true;
            int min_val = 100000;
            for(int i=0; i<lists.size(); i++){
                if(lists[i]){
                    if(lists[i]->val <= min_val){
                        min_val = lists[i]->val;
                        index = i;
                        flag = false;
                    }
                }
            }
            if(flag){
                cur->next = NULL;
                break;
            }
            ListNode* node = new ListNode(lists[index]->val);
            cur->next = node;
            cur = cur->next;
            lists[index] = lists[index]->next;
        }
        return head->next;
    }
};
```



## 24. Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head.

You may **not** modify the values in the list's nodes, only nodes itself may be changed.

**Example:**

```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

**CODE**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(!head || !head->next) return head;
        ListNode* temp;
        temp = head->next;
        head->next = swapPairs(head->next->next);
        temp->next = head;
        return temp;
    }
};
```



## 25. Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list *k* at a time and return its modified list.

*k* is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of *k* then left-out nodes in the end should remain as it is.



**Example:**

Given this linked list: `1->2->3->4->5`

For *k* = 2, you should return: `2->1->4->3->5`

For *k* = 3, you should return: `3->2->1->4->5`

**Note:**

- Only constant extra memory is allowed.
- You may not alter the values in the list's nodes, only nodes itself may be changed.

**CODE**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        
    }
};
```



## 26. Remove Duplicates from Sorted Array

Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

**Example 1:**

```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**CODE**

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty())
            return 0;
        if (nums.size() == 1)
		    return 1;
        vector<int>::iterator i;
        i = nums.begin();
        while (i < nums.end()-1) {
            if (*i == *(i+1)) 
                i = nums.erase(i);
            else
                i += 1;
        }
        return nums.size();
    }
};
```



## 27. Remove Element

Given an array *nums* and a value *val*, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example 1:**

```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeElement(nums, val);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**CODE**

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        vector<int>::iterator i;
        i = nums.begin();
        while(i < nums.end()){
            if(*i == val)
                i = nums.erase(i);
            else
                i++;
        }
        return nums.size();
    }
};
```



## 28. Implement strStr()

Implement [strStr()](http://www.cplusplus.com/reference/cstring/strstr/).

Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

**Example 1:**

```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**

```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```

**Clarification:**

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's [strstr()](http://www.cplusplus.com/reference/cstring/strstr/) and Java's [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)).

**CODE**

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(needle.length() == 0)
            return 0;
        for(int i=0; i<haystack.length(); i++){
            int index = 0, j = i;
            while(index < needle.length()){
                if(j<haystack.length() && haystack[j] == needle[index]){
                    j++;
                    index++;
                }
                else{
                    break;
                }
            }
            if(index == needle.length())
                return j - index;
        }
        return -1;
    }
};
```



## 29. Divide Two Integers

Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero.

**Example 1:**

```
Input: dividend = 10, divisor = 3
Output: 3
```

**Example 2:**

```
Input: dividend = 7, divisor = -3
Output: -2
```

**Note:**

- Both dividend and divisor will be 32-bit signed integers.
- The divisor will never be 0.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31^,  2^31^ − 1]. For the purpose of this problem, assume that your function returns 2^31^ − 1 when the division result overflows.

**CODE**

```cpp
class Solution {
public:
    int divide(int dividend, int divisor) {
        if (dividend == INT_MIN && divisor == -1) {
            return INT_MAX;
        }
        long dvd = labs(dividend), dvs = labs(divisor), ans = 0;
        int sign = dividend > 0 ^ divisor > 0 ? -1 : 1;
        while (dvd >= dvs) {
            long temp = dvs, m = 1;
            while (temp << 1 <= dvd) {
                temp <<= 1;
                m <<= 1;
            }
            dvd -= temp;
            ans += m;
        }
        return sign * ans;
    }
};
```



## 30. Substring with Concatenation of All Words

You are given a string, **s**, and a list of words, **words**, that are all of the same length. Find all starting indices of substring(s) in **s** that is a concatenation of each word in **words** exactly once and without any intervening characters.

 

**Example 1:**

```
Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
```

**Example 2:**

```
Input:
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
Output: []
```

**CODE**

```c++
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        vector<int> ans;
        if(words.size() == 0)
            return ans;
        int len = words[0].size();
        int window = len * words.size();
        if(s.size() < window)
            return ans;
        map<string, int> dict;
        for(int i=0; i<words.size(); i++)
            dict[words[i]] += 1;

        for(int i=0; i+window<=s.size(); i++){
            bool flag = true;
            vector<string> tmp = words;
            map<string, int> m_tmp = dict;
            for(int j=0; j<words.size(); j++){
                string sub = s.substr(i+j*len, len);
                if (m_tmp.find(sub) == m_tmp.end()|| m_tmp[sub] == 0){
                    flag = false;
                    break;
                }
                else
                    m_tmp[sub] -= 1;
            }
            if(flag)
                ans.push_back(i);
        }
        return ans;
    }
};
```



## 31. Next Permutation

Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be **[in-place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

**CODE**

```c++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int i = nums.size() - 2;
        while (i >= 0 && nums[i + 1] <= nums[i]) {
            i--;
        }
        if (i >= 0) {
            int j = nums.size() - 1;
            while (j >= 0 && nums[j] <= nums[i]) {
                j--;
            }
            swap(nums, i, j);
        }
        reverse(nums, i + 1);
    }

private:
    void swap(vector<int>& nums, int i, int j){
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }

    void reverse(vector<int>& nums, int start) {
        int i = start, j = nums.size() - 1;
        while (i < j) {
            swap(nums, i, j);
            i++;
            j--;
        }
    }
};
```



## 32. Longest Valid Parentheses

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

**Example 1:**

```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```

**Example 2:**

```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

**CODE**

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> char_stact;
        if (s.size() <= 1)
            return 0;
        int ans = 0;
        char_stact.push(-1);
        for(int i=0; i<s.size(); i++){
            if(s[i] == '(')
               char_stact.push(i);
            else{
                char_stact.pop();
                if(!char_stact.empty()){
                    ans  = max(ans, i - char_stact.top());
                }
                else{
                    char_stact.push(i);
                }
            }
        }
        return ans;
    }
};
```



## 33. Search in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**CODE**

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1, mid;
        while(l <= r){
            mid = (r - l) / 2 + l;
            if(nums[mid] == target)
                return mid;
            if(nums[mid] < nums[r]){
                if(nums[mid] < target && target <= nums[r])
                    l = mid + 1;
                else
                    r = mid - 1;
            }
            else{
                if(nums[l] <= target && target < nums[mid])
                    r = mid - 1;
                else
                    l = mid + 1;
            }
        }
        return -1;
    }
};
```



## 34. Find First and Last Position of Element in Sorted Array

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

If the target is not found in the array, return `[-1, -1]`.

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

**CODE**

```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> ans {nums.size(), -1};
        searchRangeFunc(nums, 0, nums.size() - 1, target, ans);
        if(ans[0] == nums.size())
            ans[0] = -1;
        return ans;
    }
    vector<int> searchRangeFunc(vector<int>& nums, int l, int r,int target, vector<int>& ans){
        if(l > r){
            return ans;
        }
        if(r - l == 1){
            if(nums[l] == target){
                ans[0] = min(l, ans[0]);
                ans[1] = max(l, ans[1]);
            }
            if(nums[r] == target){
                if(ans[0] == nums.size())
                    ans[0] = ans[1] == -1 ? r : min(r, ans[1]);
                ans[1] = max(r, ans[1]);
            }
            return ans;
        }

        int mid = (r - l) / 2 + l;
        if (nums[mid] == target){
            ans[0] = min(Solution::searchRangeFunc(nums, l, mid - 1, target, ans)[0], mid);
            ans[1] = max(Solution::searchRangeFunc(nums, mid + 1, r, target, ans)[1], mid);
        }
        if(nums[mid] < target){
            Solution::searchRangeFunc(nums, mid+1, r, target, ans);
        }
        if(nums[mid] > target){
            Solution::searchRangeFunc(nums, l, mid-1, target, ans);
        }
        return ans;
    }
};
```



## 35. Search Insert Position

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2:**

```
Input: [1,3,5,6], 2
Output: 1
```

**Example 3:**

```
Input: [1,3,5,6], 7
Output: 4
```

**Example 4:**

```
Input: [1,3,5,6], 0
Output: 0
```

**CODE**

```C++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        if(nums.size() == 0)
            return 0;
        if(nums.size() == 1){
            if(nums[0] == target || nums[0] > target)
                return 0;
            if(nums[0] < target)
                return 1;
        }
        int l = 0, r = nums.size() - 1, ans, mid;
        while(l <= r){
            mid = (r - l) / 2 + l;
            if(nums[mid] == target)
                return mid;
            if(nums[mid] < target)
                l = mid + 1;
            if(nums[mid] > target)
                r = mid - 1;
        }
        if(nums[mid] < target)
            ans = mid + 1;
        if(nums[mid] > target)
            ans = max(mid, 0);
        return ans;
    }
};
```



## 36. Valid Sudoku

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the 9 `3x3` sub-boxes of the grid must contain the digits `1-9` without repetition.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)
A partially filled sudoku which is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character `'.'`.

**Example 1:**

```
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```

**Example 2:**

```
Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being 
    modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.
- The given board contain only digits `1-9` and the character `'.'`.
- The given board size is always `9x9`.

**CODE**

```C++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        map<char, int> tmp;
        vector<map<char, int>> row_dict(9, tmp);
        vector<map<char, int>> col_dict(9, tmp);
        vector<map<char, int>> tmp2(3, tmp);
        vector<vector<map<char, int>>> box_dict(3, tmp2);

        for(int i=0; i<board.size(); i++){
            map<char, int> row;
            for(int j=0; j<board[0].size(); j++){
                map<char, int> col;
                row_dict[i][board[i][j]] += 1;
                col_dict[j][board[i][j]] += 1;
                box_dict[i/3][j/3][board[i][j]] += 1;
                if(board[i][j] != '.' && (row_dict[i][board[i][j]]>1 || col_dict[j][board[i][j]]>1 || box_dict[i/3][j/3][board[i][j]]>1)){
                    return false;
                }
            }
        }
        return true;
    }
};
```



## 37. Sudoku Solver

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

Empty cells are indicated by the character `'.'`.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)
A sudoku puzzle...

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)
...and its solution numbers marked in red.

**Note:**

- The given board contain only digits `1-9` and the character `'.'`.
- You may assume that the given Sudoku puzzle will have a single unique solution.
- The given board size is always `9x9`.

**CODE**

```C++
class Solution {
public:
    bool col[10][10],row[10][10],f[10][10];
    bool flag = false;
    void solveSudoku(vector<vector<char>>& board) {
         memset(col,false,sizeof(col));
         memset(row,false,sizeof(row));
         memset(f,false,sizeof(f));
         for(int i = 0; i < 9;i++){
             for(int j = 0; j < 9;j++){
                 if(board[i][j] == '.')   continue;
                 int temp = 3*(i/3)+j/3;
                 int num = board[i][j]-'0';
                 col[j][num] = row[i][num] = f[temp][num] = true;
             }
         }
         dfs(board,0,0);
    }
    void dfs(vector<vector<char>>& board,int i,int j){
        if(flag == true)  return ;
        if(i >= 9){
            flag = true;
            return ;
        }
        if(board[i][j] != '.'){
             if(j < 8)  dfs(board,i,j+1);
             else dfs(board,i+1,0);
             if(flag)  return;
        }
        
        else{
            int temp = 3*(i/3)+j/3;
            for(int n = 1; n <= 9; n++){
                if(!col[j][n] && !row[i][n] && !f[temp][n]){
                    board[i][j] = n + '0';
                    col[j][n] = row[i][n] = f[temp][n] = true;
                    if(j < 8)  dfs(board,i,j+1);
                    else dfs(board,i+1,0);
                    col[j][n] = row[i][n] = f[temp][n] = false;
                    if(flag)  return;
                }
            }
            board[i][j] = '.';
        }
    }
};
```



## 38. Count and Say

The count-and-say sequence is the sequence of integers with the first five terms as following:

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2`, then `one 1"` or `1211`.

Given an integer *n* where 1 ≤ *n* ≤ 30, generate the *n*th term of the count-and-say sequence. You can do so recursively, in other words from the previous member read off the digits, counting the number of digits in groups of the same digit.

Note: Each term of the sequence of integers will be represented as a string.

**Example 1:**

```
Input: 1
Output: "1"
Explanation: This is the base case.
```

**Example 2:**

```
Input: 4
Output: "1211"
Explanation: For n = 3 the term was "21" in which we have two groups "2" and "1", "2" can be read as "12" which means frequency = 1 and value = 2, the same way "1" is read as "11", so the answer is the concatenation of "12" and "11" which is "1211".
```

**CODE**

```c++
class Solution {
public:
    string countAndSay(int n) {
        if(n==1) return "1"; // base case
        string res,tmp = countAndSay(n-1); // recursion
        char c= tmp[0];
        int count=1;
        for(int i=1;i<tmp.size();i++)
            if(tmp[i]==c)
                count++;
            else {
                res+=to_string(count);
                res.push_back(c);
                c=tmp[i];
                count=1;
            }
        res+=to_string(count);
        res.push_back(c);
        return res;
    }
};
```



## 39. Combination Sum

Given a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

**CODE**

```C++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans, res;
        vector<int> tmp,tmp1;
        Solution::combinationSumFunc(candidates, target, ans, tmp);

        set<vector<int>> temp;
        for(int i=0; i<ans.size(); i++){
            sort(ans[i].begin(), ans[i].end());
            temp.insert(ans[i]);
        }
        set<vector<int>>::iterator j;
        for(j=temp.begin(); j!=temp.end(); j++){
            res.push_back(*j);
        }
        return res;
    }
    void combinationSumFunc(vector<int> arr, int target, vector<vector<int>>& ans,vector<int>& tmp){
        if(target == 0)
            return ans.push_back(tmp);
        if(target >= 0){
            for(int i=0; i<arr.size(); i++){
                tmp.push_back(arr[i]);
                combinationSumFunc(arr, target - arr[i], ans, tmp);
                tmp.pop_back();
            }
        }
    }
};
```





## 92. Reverse Linked List II

Reverse a linked list from position *m* to *n*. Do it in one-pass.

**Note:** 1 ≤ *m* ≤ *n* ≤ length of list.

**Example:**

```
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
```

**CODE**

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseBetween(self, head, m, n):
        """
        :type head: ListNode
        :type m: int
        :type n: int
        :rtype: ListNode
        """
        node=head
        prev=None
        index = 1
        #pass to the first node
        while(node):
            if index == m:
                break
            prev=node
            node=node.next
            index += 1
        #case1:
        if n-m==0:
            pass
        #case2:
        elif n-m==1:
            a=node
            b=a.next
            c=b.next if b else None
            #first node to next.next
            a.next=c
            #change b to a
            b.next=a
            #if prev exists, to b
            if prev:
                prev.next=b
            #else b is the head node
            else:
                head=b
           
        else:
            #finish reverse
            a=node
            b=a.next
            c=b.next if b else None
            while(index<n):
                b.next=a
                a=b
                b=c
                c=c.next if b else None
                index += 1
            #change first and last node link to
            if prev:
                prev.next=a
            else:
                head =a
            node.next=b
        return head
        
```



## 95. Unique Binary Search Trees II

Given an integer *n*, generate all structurally unique **BST's** (binary search trees) that store values 1 ... *n*.

**Example:**

```
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

**CODE**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def generateTrees(self, n):
        """
        :type n: int
        :rtype: List[TreeNode]
        """
        
        
        
```



## 118. Pascal's Triangle

Given a non-negative integer *numRows*, generate the first *numRows* of Pascal's triangle.

![img](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)
In Pascal's triangle, each number is the sum of the two numbers directly above it.

**Example:**

```
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

**CODE**

```C++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans;
        if(numRows <= 0)
            return ans;
        vector<int> tmp {1};
        ans.push_back(tmp);
        if(numRows == 1)
            return ans;
        
        tmp.push_back(1);
        ans.push_back(tmp);
        for(int i=2; i<numRows; i++){
            vector<int> tmp (i+1, 0);
            tmp[0] = 1, tmp[i] = 1;
            for(int j=1; j<i; j++){
                tmp[j] = ans[i-1][j-1]+ans[i-1][j];
            }
            ans.push_back(tmp);
        }
        return ans;
    }
};
```



## 206. Reverse Linked List

Reverse a singly linked list.

**Example:**

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

**Follow up:**

A linked list can be reversed either iteratively or recursively. Could you implement both?

**CODE**

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head is None:
            return None
        if head.next is None:
            return head
        if head.next.next is None:
            a=head
            b=head.next
            b.next=a
            a.next=None
            return b
        a=head
        b=a.next
        c=b.next
        a.next=None
        while(c):
            b.next=a
            a=b
            b=c
            c=c.next
        b.next = a
        return b
```



## 236. Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow **a node to be a descendant of itself**).”

Given the following binary tree: root = [3,5,1,6,2,0,8,null,null,7,4]

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

 

**Example 1:**

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**CODE**

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* ans = NULL;
    bool findNode(TreeNode* node, TreeNode* p, TreeNode* q){
        if (node == NULL)
            return false;
        int left = findNode(node->left, p, q) ? 1 : 0;
        int right = findNode(node->right, p, q) ? 1 : 0;
        int thisNode = ((node == p) || (node == q)) ? 1 : 0;
        if(left + right + thisNode >= 2)
            this->ans = node;
        return (left + right + thisNode > 0);
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        this->findNode(root, p, q);
        return ans;
    }
};
```





## 310. Minimum Height Trees

For an undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees (MHTs). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

**Format**
The graph contains `n` nodes which are labeled from `0` to `n - 1`. You will be given the number `n` and a list of undirected `edges` (each edge is a pair of labels).

You can assume that no duplicate edges will appear in `edges`. Since all edges are undirected, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in `edges`.

**Example 1 :**

```
Input: n = 4, edges = [[1, 0], [1, 2], [1, 3]]

        0
        |
        1
       / \
      2   3 

Output: [1]
```

**Example 2 :**

```
Input: n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

     0  1  2
      \ | /
        3
        |
        4
        |
        5 

Output: [3, 4]
```

**Note**:

- According to the [definition of tree on Wikipedia](https://en.wikipedia.org/wiki/Tree_(graph_theory)): “a tree is an undirected graph in which any two vertices are connected by *exactly* one path. In other words, any connected graph without simple cycles is a tree.”
- The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

**CODE**

```python
class Solution(object):
    def findMinHeightTrees(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: List[int]
        """
        tree=[[] for _ in range(n)]
        for s,d in edges:
            tree[s].append(d)
            tree[d].append(s)
        outside=[i for i in range(n) if len(tree[i]) < 2]
        inside=[]
        while(1):
            for node in outside:
                for c_node in tree[node]:
                    tree[c_node].remove(node)
                    if len(tree[c_node])==1:
                        inside.append(c_node)
            if not inside:
                break
            outside=inside
            inside=[]
        return outside
```



## 563. Binary Tree Tilt

Given a binary tree, return the tilt of the **whole tree**.

The tilt of a **tree node** is defined as the **absolute difference** between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.

The tilt of the **whole tree** is defined as the sum of all nodes' tilt.

**Example:**

```
Input: 
         1
       /   \
      2     3
Output: 1
Explanation: 
Tilt of node 2 : 0
Tilt of node 3 : 0
Tilt of node 1 : |2-3| = 1
Tilt of binary tree : 0 + 0 + 1 = 1
```

**Note:**

1. The sum of node values in any subtree won't exceed the range of 32-bit integer.
2. All the tilt values won't exceed the range of 32-bit integer.

**CODE**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    ans = 0
    def findTilt(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        self.addTilt(root)
        return self.ans
        
    def addTilt(self, node):
        if node is None:
            return 0
        left = self.addTilt(node.left)
        right = self.addTilt(node.right)
        self.ans += abs(left - right)
        #description of this problem is not correct, it doesn't mention add node.val
        return left+right+node.val
```





## 572. Subtree of Another Tree

Given two non-empty binary trees **s** and **t**, check whether tree **t** has exactly the same structure and node values with a subtree of **s**. A subtree of **s** is a tree consists of a node in **s** and all of this node's descendants. The tree **s** could also be considered as a subtree of itself.

**Example 1:**
Given tree s:

```
     3
    / \
   4   5
  / \
 1   2
```

Given tree t:

```
   4 
  / \
 1   2
```

Return 

true

, because t has the same structure and node values with a subtree of s.

**Example 2:**
Given tree s:

```
     3
    / \
   4   5
  / \
 1   2
    /
   0
```

Given tree t:

```
   4
  / \
 1   2
```

Return 

false

**CODE**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSub(self,s,t):
        if t is None and s is None:
            return True
        if (t and not s) or (not t and s) :
            return False
        if s.val != t.val:
            return False
        return self.isSub(s.left,t.left) and self.isSub(s.right,t.right)
    
    def isSubtree(self, s, t):
        """
        :type s: TreeNode
        :type t: TreeNode
        :rtype: bool
        """
        if t is None:
            return True
        if s is None:
            return False
        res = False
        if t.val == s.val:
            res = self.isSub(s,t)
        if not res:
            res = self.isSubtree(s.left,t)
        if not res:
            res = self.isSubtree(s.right,t)
        return res
```



## 654. Maximum Binary Tree

Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:

1. The root is the maximum number in the array.
2. The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.
3. The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.



Construct the maximum tree by the given array and output the root node of this tree.

**Example 1:**

```
Input: [3,2,1,6,0,5]
Output: return the tree root node representing the following tree:

      6
    /   \
   3     5
    \    / 
     2  0   
       \
        1
```

**Note:**

1. The size of the given array will be in the range [1,1000].

**CODE**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def constructMaximumBinaryTree(self, nums):
        """
        :type nums: List[int]
        :rtype: TreeNode
        """
        if len(nums) == 0:
            return
        root = TreeNode(max(nums))
        left = nums[:nums.index(max(nums))]
        root.left = self.constructMaximumBinaryTree(left)
        right = nums[nums.index(max(nums))+1:]
        root.right = self.constructMaximumBinaryTree(right)
        return root
```



## 655. Print Binary Tree

Print a binary tree in an m*n 2D string array following these rules:

1. The row number `m` should be equal to the height of the given binary tree.
2. The column number `n` should always be an odd number.
3. The root node's value (in string format) should be put in the exactly middle of the first row it can be put. The column and the row where the root node belongs will separate the rest space into two parts (**left-bottom part and right-bottom part**). You should print the left subtree in the left-bottom part and print the right subtree in the right-bottom part. The left-bottom part and the right-bottom part should have the same size. Even if one subtree is none while the other is not, you don't need to print anything for the none subtree but still need to leave the space as large as that for the other subtree. However, if two subtrees are none, then you don't need to leave space for both of them.
4. Each unused space should contain an empty string `""`.
5. Print the subtrees following the same rules.

**Example 1:**

```
Input:
     1
    /
   2
Output:
[["", "1", ""],
 ["2", "", ""]]
```



**Example 2:**

```
Input:
     1
    / \
   2   3
    \
     4
Output:
[["", "", "", "1", "", "", ""],
 ["", "2", "", "", "", "3", ""],
 ["", "", "4", "", "", "", ""]]
```



**Example 3:**

```
Input:
      1
     / \
    2   5
   / 
  3 
 / 
4 
Output:

[["",  "",  "", "",  "", "", "", "1", "",  "",  "",  "",  "", "", ""]
 ["",  "",  "", "2", "", "", "", "",  "",  "",  "",  "5", "", "", ""]
 ["",  "3", "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]
 ["4", "",  "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]]
```

**Note:** The height of binary tree is in the range of [1, 10].

**CODE**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def printTree(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[str]]
        """
        if root is None:
            return []
        ans = []
        queue = [(1,root)]
        deep=1
        none_cot=0
        while(len(queue)>0):
            tmp=0
            node = queue[0]
            queue.pop(0)
            
            if node[1] is None:
                ans.append((node[0],""))
                #continue
            else:
                ans.append((node[0],str(node[1].val)))
                
                if node[1].left or node[1].right:
                    tmp=node[0]+1
                    deep = max(deep, tmp)
            tmp=node[0]+1
            if node[1]:
                none_cot=0
                if node[1].left:
                    left = (tmp,node[1].left)
                else:
                    left = (tmp,None)
                queue.append(left)

                if node[1].right:
                    right = (tmp,node[1].right)
                else:
                    right = (tmp,None)
                queue.append(right)
            else:
                none_cot+=1
                if none_cot>2**deep:
                    break
                left = (tmp,None)
                right = (tmp,None)
                queue.append(left)
                queue.append(right)
        	
        #print this tree    
        res = [[""]*(2**deep-1) for i in range(deep)]
        #(x,y,val)
        grid = [[0,(2**deep-1)//2,ans[0][1]]]
        for i in range(1,deep):
            s=2**i-1
            for j in range(s,s+2**i,2):
                y = (2**(deep-i)-1)//2
                father = grid[(j-1)/2]
                grid.append((ans[j][0]-1,father[1]-y-1,ans[j][1]))
                grid.append((ans[j+1][0]-1,father[1]+y+1,ans[j+1][1]))
        #return grid
        for i in grid:
            res[i[0]][i[1]]=i[2]
        return res
```



## 662. Maximum Width of Binary Tree

Given a binary tree, write a function to get the maximum width of the given tree. The width of a tree is the maximum width among all levels. The binary tree has the same structure as a **full binary tree**, but some nodes are null.

The width of one level is defined as the length between the end-nodes (the leftmost and right most non-null nodes in the level, where the `null` nodes between the end-nodes are also counted into the length calculation.

**Example 1:**

```
Input: 

           1
         /   \
        3     2
       / \     \  
      5   3     9 

Output: 4
Explanation: The maximum width existing in the third level with the length 4 (5,3,null,9).
```

**Example 2:**

```
Input: 

          1
         /  
        3    
       / \       
      5   3     

Output: 2
Explanation: The maximum width existing in the third level with the length 2 (5,3).
```

**Example 3:**

```
Input: 

          1
         / \
        3   2 
       /        
      5      

Output: 2
Explanation: The maximum width existing in the second level with the length 2 (3,2).
```

**Example 4:**

```
Input: 

          1
         / \
        3   2
       /     \  
      5       9 
     /         \
    6           7
Output: 8
Explanation:The maximum width existing in the fourth level with the length 8 (6,null,null,null,null,null,null,7).
```

**Note:** Answer will in the range of 32-bit signed integer.

**CODE**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> left;
    vector<int> right;
    int widthOfBinaryTree(TreeNode* root) {
		left_dfs(root);
        right_dfs(root);
        level = min(left.size(),right.size());
        return pow(2,level-1);
    }
    int left_dfs(TreeNode* node) {
        if (node == NULL){
            return 0;
        }
        left.push_back(node->val);
        left_dfs(node->left);
    }
    int right_dfs(TreeNode* node) {
        if (node == NULL){
            return 0;
        }
        right.push_back(node->val);
        right_dfs(node->right); 
    }
};
```





## 965. Univalued Binary Tree

A binary tree is *univalued* if every node in the tree has the same value.

Return `true` if and only if the given tree is univalued.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_1.png)



```
Input: [1,1,1,1,1,null,1]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/28/unival_bst_2.png)

```
Input: [2,2,2,5,2]
Output: false
```

 

**Note:**

1. The number of nodes in the given tree will be in the range `[1, 100]`.
2. Each node's value will be an integer in the range `[0, 99]`.

**CODE**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isUnivalTree(TreeNode* root) {
        return preOrder(root);
    }
    
    bool preOrder(TreeNode* root){
        if (!root)
            return false;
        stack<TreeNode*> s;
        s.push(root);
        int b = root->val;
        while (!s.empty()){
            TreeNode* node = s.top();
            s.pop();
            if (node->val != b)
                return false;
            if (node->left)
                s.push(node->left);
            if (node->right)
                s.push(node->right);
        }
        return true;
    }
};
```



## 979. Distribute Coins in Binary Tree

Given the `root` of a binary tree with `N` nodes, each `node` in the tree has `node.val` coins, and there are `N` coins total.

In one move, we may choose two adjacent nodes and move one coin from one node to another.  (The move may be from parent to child, or from child to parent.)

Return the number of moves required to make every node have exactly one coin.

 

**Example 1:**

**![img](https://assets.leetcode.com/uploads/2019/01/18/tree1.png)**

```
Input: [3,0,0]
Output: 2
Explanation: From the root of the tree, we move one coin to its left child, and one coin to its right child.
```

**Example 2:**

**![img](https://assets.leetcode.com/uploads/2019/01/18/tree2.png)**

```
Input: [0,3,0]
Output: 3
Explanation: From the left child of the root, we move two coins to the root [taking two moves].  Then, we move one coin from the root of the tree to the right child.
```

**Example 3:**

**![img](https://assets.leetcode.com/uploads/2019/01/18/tree3.png)**

```
Input: [1,0,2]
Output: 2
```

**Example 4:**

**![img](https://assets.leetcode.com/uploads/2019/01/18/tree4.png)**

```
Input: [1,0,0,null,3]
Output: 4
```

 

**Note:**

1. `1<= N <= 100`
2. `0 <= node.val <= N`

**CODE**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int ans = 0;
    int distributeCoins(TreeNode* root) {
        dfs(root);
        return ans;
    }
    int dfs(TreeNode* node){
        if (node == NULL){
            return 0;
        }
        int L = dfs(node->left);
        int R = dfs(node->right);
        ans += abs(L)+abs(R);
        return node->val + L + R - 1;
    }
};
```

