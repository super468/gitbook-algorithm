---
description: 'Leetcode 139 https://leetcode.com/problems/word-break/'
---

# Word Break

Given a **non-empty** string _s_ and a dictionary _wordDict_ containing a list of **non-empty** words, determine if _s_ can be segmented into a space-separated sequence of one or more dictionary words.

**Note:**

* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.

**Example 1:**

```text
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**

```text
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```text
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

## 思路

这道题首先很经典，如果是典型的用memory去优化recursion的方法。首先用recursion，就是string左边开始去匹配wordDictionary里的Word，如果存在的，就去递归判断剩下的string是不是wordbreakable。总的时间复杂度为O\(2^n\)。后面解释。因为这里面存在着很多重复的计算，所以我们需要memory去存计算的结果，避免重复计算。用一个memo 数组去存。

## 复杂度

没有memo:

时间复杂度：O\(2^N\)

[https://leetcode.com/problems/word-break/discuss/169383/The-Time-Complexity-of-The-Brute-Force-Method-Should-Be-O\(2n\)-and-Prove-It-Below](https://leetcode.com/problems/word-break/discuss/169383/The-Time-Complexity-of-The-Brute-Force-Method-Should-Be-O%282n%29-and-Prove-It-Below)

空间复杂度：O\(N\)

有memo:

时间复杂度：O\(N^2\)

空间复杂度：O\(N\)

## 代码

### brute force

```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        return word_Break(s, new HashSet(wordDict), 0);
    }
    public boolean word_Break(String s, Set<String> wordDict, int start) {
        if (start == s.length()) {
            return true;
        }
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end)) && word_Break(s, wordDict, end)) {
                return true;
            }
        }
        return false;
    }
}
```

### recursion with memo

```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        return word_Break(s, new HashSet(wordDict), 0, new Boolean[s.length()]);
    }
    public boolean word_Break(String s, Set<String> wordDict, int start, Boolean[] memo) {
        if (start == s.length()) {
            return true;
        }
        if (memo[start] != null) {
            return memo[start];
        }
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end)) && word_Break(s, wordDict, end, memo)) {
                return memo[start] = true;
            }
        }
        return memo[start] = false;
    }
}
```

### BFS

```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet=new HashSet(wordDict);
        Queue<Integer> queue = new LinkedList<>();
        int[] visited = new int[s.length()];
        queue.add(0);
        while (!queue.isEmpty()) {
            int start = queue.remove();
            if (visited[start] == 0) {
                for (int end = start + 1; end <= s.length(); end++) {
                    if (wordDictSet.contains(s.substring(start, end))) {
                        queue.add(end);
                        if (end == s.length()) {
                            return true;
                        }
                    }
                }
                visited[start] = 1;
            }
        }
        return false;
    }
}
```

### DP

```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet=new HashSet(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
```

## Follow up: Word Break II

Return all such possible sentences.

思路是一样的不过是吧memo记录的东西改一下，把存true/false改成存list of string

### Recursion with memo

```java
public class Solution {

    public List<String> wordBreak(String s, Set<String> wordDict) {
        return word_Break(s, wordDict, 0);
    }
    HashMap<Integer, List<String>> map = new HashMap<>();

    public List<String> word_Break(String s, Set<String> wordDict, int start) {
        if (map.containsKey(start)) {
            return map.get(start);
        }
        LinkedList<String> res = new LinkedList<>();
        if (start == s.length()) {
            res.add("");
        }
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end))) {
                List<String> list = word_Break(s, wordDict, end);
                for (String l : list) {
                    res.add(s.substring(start, end) + (l.equals("") ? "" : " ") + l);
                }
            }
        }
        map.put(start, res);
        return res;
    }
}
```

### DP

过不了最后一个case。。。

```java
class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {
        HashMap<Integer, List<String>> map = new HashMap<>();
        HashSet<String> set = new HashSet<>(wordDict);
        // dp[i] = true if dp[j] == true %% substring(j, i) is valid
        map.put(0, new ArrayList<>(Arrays.asList("")));
        for(int i = 1; i <= s.length(); i++){
            List<String> res = new ArrayList<>();
            for(int j = 0; j < i; j++){
                String word = s.substring(j, i);
                if(map.get(j).size() > 0 && set.contains(word)){
                    for(String str : map.get(j)){
                        res.add(str + (!str.equals("") ? " " : "") + word);
                    }
                }
            }
            map.put(i, res);
        }
        return map.get(s.length());
    }
}
```

