---
description: 'Leetcode 127 https://leetcode.com/problems/word-ladder/'
---

# Word Ladder \(TODO\)

Given two words \(_beginWord_ and _endWord_\), and a dictionary's word list, find the length of shortest transformation sequence from _beginWord_ to _endWord_, such that:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in the word list. Note that _beginWord_ is _not_ a transformed word.

**Note:**

* Return 0 if there is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.
* You may assume no duplicates in the word list.
* You may assume _beginWord_ and _endWord_ are non-empty and are not the same.

**Example 1:**

```text
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

**Example 2:**

```text
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

## 复杂度

* 时间 O\(n \* 26 \* L\) n 是所有的单词，L是单词的长度
* 空间 O\(n \* 26 \* L\) 对n中的每个word来存储他的26 \* L的变种

## 代码

### BFS

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Queue<String> queue = new LinkedList<>();
        // all words here are not visited
        Set<String> set = new HashSet<>(wordList);
        int ladder = 0;
        queue.offer(beginWord);
        while(!queue.isEmpty()){
            for(int k = queue.size(); k > 0; k--){
                String top = queue.poll();
                if(endWord.equals(top)) return ladder + 1;
                else {
                    for(int i = 0; i < top.length(); i++){
                        //对于每一位字符的替换都要新建一个新的数组
                        char[] chrs = top.toCharArray();
                        for(char c = 'a'; c <= 'z'; c++){
                            if(chrs[i] == c) continue;
                            else chrs[i] = c;
                            String tmp = new String(chrs);
                            if(set.contains(tmp)){
                                queue.offer(tmp);
                                set.remove(tmp);
                            }
                        }
                    }
                }
            }
            ladder++;
        }
        return 0;
    }
}
```

### Bidirectional BFS

同时从顶端和末端BFS

