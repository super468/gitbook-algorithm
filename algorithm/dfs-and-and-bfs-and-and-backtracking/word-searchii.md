---
description: LeetCode 212
---

# Word SearchII

Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example:**

```text
Input: 
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
```

**Note:**

1. All inputs are consist of lowercase letters `a-z`.
2. The values of `words` are distinct.

## 思路

作为Word Search的follow up，如果只是简单的iterate每个Word，将会造成大量的重复访问。对于相似的Word，这样做会很浪费。但是如果构建一个Trie 来表示这个Word list，我们只需要遍历棋盘，并在合适的时候对Trie 进行搜索。

## 复杂度

* 时间：O\(m \* n \* 4 ^ wl\) wl是word的平均长度
* 空间：O\(wl\) 这个也就是trie的平均深度

## 代码

有两个坑需要注意，注释里有

```java
class TrieNode{
        private static int R = 26;
        boolean isEnd;
        TrieNode[] next = new TrieNode[R];
}

class Solution {    
    public void buildTrie(String[] words){
        for(String word: words){
            TrieNode node = root;
            for(int i = 0; i < word.length(); i++){
                char c = word.charAt(i);
                if(node.next[c - 'a'] == null){
                    node.next[c - 'a'] = new TrieNode();
                }
                node = node.next[c - 'a'];
            }
            node.isEnd = true;
        }
    }
    
    TrieNode root = new TrieNode();
    public List<String> findWords(char[][] board, String[] words) {
        List<String> res = new ArrayList<>();
        if(board.length < 1 || board[0].length < 1) return res;
        buildTrie(words);
        for(int i = 0 ; i < board.length; i++){
            for(int j = 0; j < board[i].length; j++){
                backtracking(board, i , j, res, "", root);
            }
        }
        return res;
    }
    
    public void backtracking(char[][] board, int i, int j, List<String> res, String s, TrieNode node){
        if(i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] == '#' || node.next[board[i][j] - 'a'] == null) return;
        node = node.next[board[i][j] - 'a'];
        //判断成功之后一定要继续，后续可能还有其他的Word
        if(node.isEnd) {
            res.add(s + board[i][j]);
            node.isEnd = false;
        }
        char c = board[i][j];
        board[i][j] = '#';
        // 注意这里一定是 s + c，不是board[i][j]因其已改
        backtracking(board, i + 1, j, res, s + c, node);
        backtracking(board, i - 1, j, res, s + c, node);
        backtracking(board, i, j + 1, res, s + c, node);
        backtracking(board, i, j - 1, res, s + c, node);
        board[i][j] = c;
    }
}
```



