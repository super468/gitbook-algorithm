---
description: Leetcode 340
---

# Longest Substring with At Most K Distinct Characters

Given a string, find the length of the longest substring T that contains at most k distinct characters.

**Example 1:**

```text
Input: s = "eceba", k = 2
Output: 3
Explanation: T is "ece" which its length is 3.
```

**Example 2:**

```text
Input: s = "aa", k = 1
Output: 2
Explanation: T is "aa" which its length is 2.
```

思路：这题是leetcode 159的变形。主要的思想就是利用双指针维护一个sliding window。具体做法分为两种。第一种就是用hashmap来保存访问过的字符的个数，当hashmap超过K的时候，我们需要维护sliding window的合法性，就需要将left指针向左移动，来删除一个字符，达到要求。我们只需要将当前left的对应的元素的映射值减一，如果减完为0，那么后面就没有相同的元素，那么我们就可以把它从map里去除。换句话说，此时的left对应的元素是这个window里，最后出现的位置。第二种方法就是改为存最后一次访问到字符的位置，那么如果left对应最后一个访问字符的位置，我们就把这个字符删掉就可以了。对于这道题，定义一个大小为256的数组更方便一点，但是这里为了广泛性就用hashmap了。

代码1：

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        HashMap<Character, Integer> map = new HashMap<>();
        int left = 0, right = 0, max = 0;
        while(right < s.length()){
            char c = s.charAt(right);
            map.put(c, map.getOrDefault(c, 0) + 1);
            while(map.size() == k + 1){
                map.put(s.charAt(left), map.get(s.charAt(left)) - 1);
                if(map.get(s.charAt(left)) == 0) map.remove(s.charAt(left));
                left++;
            }
            max = Math.max(max, right - left + 1);
            right++;
        }
        return max;
    }
}
```

代码2：

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        HashMap<Character, Integer> map = new HashMap<>();
        int left = 0, right = 0, max = 0;
        while(right < s.length()){
            map.put(s.charAt(right), right);
            while(map.size() == k + 1){
                if(map.get(s.charAt(left)) == left) map.remove(s.charAt(left));
                left++;
            }
            max = Math.max(max, right - left + 1);
            right++;
        }
        return max;
    }
}
```



