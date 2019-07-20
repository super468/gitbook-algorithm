---
description: LeetCode 79
---

# Word Search

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**

```text
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

## 思路

对棋盘进行搜索寻找一个存在的解，而且节点可能出现在多个搜索的路径当中，这需要我们能够在搜索的时候对节点赋为visited状态，并在搜索结束reset节点的状态，以便其他搜索路径的访问，这就是Backtracking。

## 复杂度

* 时间:：我觉得是O\(m \* n \* 4 ^ l\). l是string的长度
* 空间：O\(l\) 栈的长度

## 代码

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        if(board.length < 1 || board[0].length < 1) return false;
        for(int i = 0 ; i < board.length; i++){
            for(int j = 0; j < board[i].length; j++){
                if(backtracking(board, word, 0, i, j)) return true;
            }
        }
        return false;
    }
    
    public boolean backtracking(char[][] board, String word, int index, int row, int col){
        // 判断所有不合法的情况，数组越界，节点已经访问过，以及其他为false的情况
        if(row < 0 || row >= board.length || col < 0 || col >= board[0].length || board[row][col] != word.charAt(index)) return false;
        // 判断搜索成功的情况
        if(word.length() == index) return true;
        // 搜索
        board[row][col] ^= 256;
        if(backtracking(board, word, index + 1, row - 1, col)||
           backtracking(board, word, index + 1, row + 1, col)||
           backtracking(board, word, index + 1, row, col - 1)||
           backtracking(board, word, index + 1, row, col + 1))
            return true;
        board[row][col] ^= 256;
        return false;
    }
}
```

