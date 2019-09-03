---
description: Leetcode 680
---

# Valid Palindrome II

Given a non-empty string `s`, you may delete **at most** one character. Judge whether you can make it a palindrome.

**Example 1:**  


```text
Input: "aba"
Output: True
```

**Example 2:**  


```text
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

**Note:**

1. The string will only contain lowercase characters a-z. The maximum length of the string is 50000.

## 思路

我感觉对于palindrome的情况百分之九十的解法都是用two pointers

暴力解即对于string的每一个元素，删掉它，然后判断剩下的string是否是回文。时间复杂度O\(n^2\)。考虑回文的特性，我们可以知道对于string\[i:j\]，它成为回文的条件有两个：

1. string\[i\] == string\[j\]
2. string\[i + 1 : j - 1\] 要是回文的

如果string\[i\] != string\[j\]的话，我们要删除其中一个 either string\[i\] or string\[j\]，然后再判断string\[i + 1, j\] 和string\[i, j -1\]是否为回文。

时间复杂度为O\(2 \* N\) = O\(N\)

Follow up: 如果让你最多删除K个character，怎么做？

把1扩展成K，利用recursion，如果两个字符不相等就减去一个字符，把问题转换成k - 1的问题，因为我们在每一步的时候有两个选择删除，总共的可能性就是O\(2^K\)，每个isPalindromeO\(N\)，所以最后的时间复杂度是O\(2^k \* n\)

## 复杂度

时间：O\(n\)  
空间：O\(1\)

删除K个

时间：O\(2^k \* n\)  
空间：O\(k + n\)  n是应该递归调用栈

## 代码

### 一个

```java
class Solution {
    public boolean validPalindrome(String s) {
        int l = 0, r = s.length() - 1;
        while(l < r){
            if(s.charAt(l) != s.charAt(r)) return isPalindrome(s, l + 1, r) || isPalindrome(s, l, r - 1);
            l++;
            r--;
        }
        return true;
    }

    public boolean isPalindrome(String s, int l, int r){
        while(l < r){
            if(s.charAt(l++) != s.charAt(r--)) return false;
        }
        return true;
    }
}
```

### K个

```java
class Solution {
    public boolean validPalindrome(String s) {
        return isPalindrome(s, 0, s.length() - 1, 1);
    }

    public boolean isPalindrome(String s, int i, int j, int d){
        if(i >= j) return true;
        if(s.charAt(i) == s.charAt(j)){
            return isPalindrome(s, i + 1, j - 1, d);
        } else{
            return d > 0 && (isPalindrome(s, i, j - 1, d - 1) || isPalindrome(s, i + 1, j, d - 1));
        }
    }
}
```



