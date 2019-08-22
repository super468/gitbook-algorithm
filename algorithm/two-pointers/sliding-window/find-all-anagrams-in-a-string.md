---
description: Leetcode 438
---

# Find All Anagrams in a String

Given a string **s** and a **non-empty** string **p**, find all the start indices of **p**'s anagrams in **s**.

Strings consists of lowercase English letters only and the length of both strings **s** and **p** will not be larger than 20,100.

The order of output does not matter.

**Example 1:**

```text
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```text
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

## 思路

对于在一个string里寻找substring使其满足某些特定的条件的题目，可以使用一类通用的方法，即hashmap + two pointer aka sliding window。这道题的条件是substring必须和给定的string同词异序（同序亦可）。把这个条件拆解下来就是：

1. substring必须包含给定string的所有字符
2. substring必须与给定string等长

我们可以用一个可伸缩的sliding window去找包含所有字符的substring, 然后再判断是否等长

## 复杂度

时间复杂度：O\(n\)

空间复杂度：O\(n\)

## 代码

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        // 1. sliding window is substring
        // 2. sliding window length equals to p's length
        // 3. sliding window characters == p characters
        //HashMap<Character, Integer> map = new HashMap<>();
        List<Integer> res = new ArrayList<>();
        int[] map = new int[26];
        int count = 0;
        for(char c : p.toCharArray()){
            if(map[c - 'a']++ == 0) count++;
        }
        
        int left = 0, right = 0;
        while(right < s.length()){
            char c = s.charAt(right);
            if(--map[c - 'a'] == 0) count--;
            while(count == 0){
                if(right - left + 1 == p.length()){
                    res.add(left);
                }
                if(map[s.charAt(left++) - 'a']++ == 0) count++;
            }
            right++;
        }
        return res;
    }
}
```

