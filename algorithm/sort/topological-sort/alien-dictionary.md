---
description: Leetcode 269
---

# Alien Dictionary\(TODO\)

There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of **non-empty** words from the dictionary, where **words are sorted lexicographically by the rules of this new language**. Derive the order of letters in this language.

**Example 1:**

```text
Input:
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]

Output: "wertf"
```

**Example 2:**

```text
Input:
[
  "z",
  "x"
]

Output: "zx"
```

**Example 3:**

```text
Input:
[
  "z",
  "x",
  "z"
] 

Output: "" 

Explanation: The order is invalid, so return "".
```

**Note:**

1. You may assume all letters are in lowercase.
2. You may assume that if a is a prefix of b, then a must appear before b in the given dictionary.
3. If the order is invalid, return an empty string.
4. There may be multiple valid order of letters, return any one of them is fine.

## 思路

亚麻VO原题，当时没印象做过这道题，挂了。痛定思痛。其实这是一道topological sort的题。对于排好序的单词，我们可以通过adjacent words 获取到一些单词的前后顺序。利用这些信息来构建一个graph。

如果是用BFS来做topological sort的话，需要同时记录一下单词的indegree。如果是用DFS来做的话，由于 DFS 遍历需要标记遍历结点，那么就用一个 visited 数组，由于是深度优先的遍历，我们并不需要一定要从入度为0的结点开始遍历，而是从任意一个结点开始都可以，DFS 会遍历到出度为0的结点为止，加入结果 res，然后回溯加上整条路径到结果 res 即可。DFS的一大特点就是，当前节点下的所有节点都访问完毕后才会回溯回当前节点。

## 复杂度

时间：O\(n\)  
空间：O\(n\)

## 易错点

1. toString substring 大小写
2. cur\[j\] != next\[j\] 默认是cur\[j\] &lt; next\[j\]
3. duplicate edges 不要重复加indegree
4. 从graph的所有节点里所有入度为0的点，包括孤岛
5. 不用设置visited数组，因为数组有环的话，最后结果的长度小于graph中node的数量

## 代码

### BFS

```java
class Solution {
    public String alienOrder(String[] words) {
        //set the indegree
        HashMap<Character, Integer> map = new HashMap<>();
        for(String str : words){
            for(char c : str.toCharArray()){
                map.put(c, 0);
            }
        }
        HashMap<Character, Set<Character>> graph = new HashMap<>();
        //build the graph
        for(int i = 0; i < words.length - 1; i++){
            char[] cur = words[i].toCharArray();
            char[] next = words[i + 1].toCharArray();
            for(int j = 0; j < cur.length; j++){
                if(cur[j] != next[j]){
                    if(!graph.containsKey(cur[j])) graph.put(cur[j], new HashSet<Character>());
                    if(!graph.get(cur[j]).contains(next[j])){
                        graph.get(cur[j]).add(next[j]);
                        map.put(next[j], map.get(next[j]) + 1);
                    }
                    break;
                }
            }
        }
        //bfs
        Queue<Character> q = new LinkedList<Character>();
        for(char c : map.keySet()){
            if(map.get(c) == 0) q.offer(c);
        }
        StringBuilder sb = new StringBuilder();
        while(!q.isEmpty()){
            char p = q.poll();
            sb.append(p);
            if(graph.containsKey(p)){
                for(char c : graph.get(p)){
                    map.put(c, map.get(c) - 1);
                    if(map.get(c) == 0) q.offer(c);
                }
            }
        }
        if(sb.length() != map.size()) return "";
        return sb.toString();
    }
}
```

### DFS

```java
class Solution {
    public String alienOrder(String[] words) {
        int[][] map = new int[26][26];
        for(String word : words){
            for(char c : word.toCharArray()){
                map[c - 'a'][c - 'a'] = 1;
            }
        }
        for(int i = 1; i < words.length; i++){
            String a = words[i - 1];
            String b = words[i];
            int min = Math.min(a.length(), b.length());
            int j = 0;
            while(j < min){
                if(a.charAt(j) != b.charAt(j)){
                    map[a.charAt(j) - 'a'][b.charAt(j) - 'a'] = 1;
                    break;
                }
                j++;
            }
            if(j == min && a.length() > b.length()) return "";
        }
        int[] visited = new int[26];
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < 26; i++){
            if(!DFS(map, i, visited, sb)) return "";
        }
        return sb.reverse().toString();
        
    }
    public boolean DFS(int[][] map, int cur, int[] visited, StringBuilder sb){
        if(map[cur][cur] == 0) return true;
        visited[cur] = 1;
        for(int i = 0; i < 26; i++){
            if(i == cur || map[cur][i] == 0) continue;
            if(visited[i] == 1) return false;
            if(!DFS(map, i, visited, sb)) return false;
        }
        visited[cur] = 0;
        map[cur][cur] = 0;
        sb.append((char)(cur + 'a'));
        return true;
    }
}
```

