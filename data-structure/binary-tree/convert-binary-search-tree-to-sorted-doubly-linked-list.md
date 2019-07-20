---
description: LeetCode 426
---

# Convert Binary Search Tree to Sorted Doubly Linked List

## 题目

Convert a BST to a sorted circular doubly-linked list in-place. Think of the left and right pointers as synonymous to the previous and next pointers in a doubly-linked list.

Let's take the following BST as an example, it may help you understand the problem better:  

![](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)

We want to transform this BST into a circular doubly linked list. Each node in a doubly linked list has a predecessor and successor. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

The figure below shows the circular doubly linked list for the BST above. The "head" symbol means the node it points to is the smallest element of the linked list.  

![](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)

Specifically, we want to do the transformation in place. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. We should return the pointer to the first element of the linked list.

The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship.  

![](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturnbst.png)

## 思路

Inorder Traverse的两种方法，递归和非递归。

## 复杂度：

* 时间：O\(N\)
* 空间：O\(logN\) - O\(N\) worst case completely unbalanced tree

## 代码：

### 非递归

有一个大坑就是我总是把root当成是list的第一个元素，切记它不是。

```java
class Solution {
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
        Stack<Node> stack = new Stack<>();
        Node cur = root;
        
        Node pre = null;
        Node first = null;
        
        while(cur != null || !stack.isEmpty()){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            Node top = stack.pop();
            if(first == null) first = top;
            top.left = pre;
            if(pre != null) pre.right = top;
            pre = top;
            cur = top.right;
        }
        
        pre.right = first;
        first.left = pre;
        
        return first;
    }
}
```

### 递归

```java
class Solution {
    Node first = null;
    Node last = null;
    public Node treeToDoublyList(Node root) {
        if(root == null) return null;
        helper(root);
        first.left = last;
        last.right = first;
        return first;
    }
    
    public void helper(Node node){
        if(node != null){
            helper(node.left);
            if(last != null){
                last.right = node;
                node.left = last;
            } else{
                first = node;
            }
            last = node;
            helper(node.right);
        }
    }
}
```



