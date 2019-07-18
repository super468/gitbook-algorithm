---
description: LeetCode 236
---

# Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor \(LCA\) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants \(where we allow **a node to be a descendant of itself**\).”

Given the following binary tree:  root = \[3,5,1,6,2,0,8,null,null,7,4\] ![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Example 1:**

```text
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

```text
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Note:**

* All of the nodes' values will be unique.
* p and q are different and both values will exist in the binary tree.

**思路:**

Solution里提供了三个不同的版本，recursive，iterative with parent pointer，iterative without parent pointer。

**recursive**

**分治的方法，先分开寻找 p 和 q, 然后再根据返回的结果来conquer**

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || p == null || q == null) return null;
        if(root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left != null && right != null) return root;
        if(left == null) return right;
        if(right == null) return left;
        return null;
    }
}
```

**iterative with parent pointer**

遍历寻找p 和 q，并记录下路径中各节点的父节点，保存到p 和 q 的 path，然后p的path节点变set,方便q查询

```java
class Solution {
    HashMap<TreeNode, TreeNode> map = new HashMap<>();
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || p == null || q == null) 
            return null;
        //reach to p and q
        Preorder(root, p, q);
        map.put(root, null);
        HashSet<TreeNode> set = new HashSet<>();
        while(p != null){
            set.add(p);
            p = map.get(p);
        }
        while(!set.contains(q)){
            q = map.get(q);
        }
        return q;
    }
    
    public void Preorder(TreeNode node, TreeNode p, TreeNode q){
        if(node == null) return;
        if(node.left != null){
            map.put(node.left, node);
            if(node.left != p || node.left != q)
                Preorder(node.left, p, q);
        }
        if(node.right != null){
            map.put(node.right, node);
            if(node.right != p || node.right != q)
                Preorder(node.right, p, q);
        }
    }
}
```

**iterative without parent pointer**

post traversal + potential LCA

```java
class Solution {
    HashMap<TreeNode, TreeNode> map = new HashMap<>();
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || p == null || q == null) 
            return null;
        //postorder iterative
        boolean one_node_found = false;
        TreeNode LCA = null;
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        TreeNode pre = null;
        while(cur != null || !stack.isEmpty()){
            if(cur != null){
                stack.push(cur);
                if(cur == p || cur == q){
                    if(one_node_found){
                        return LCA;
                    } else{
                        one_node_found = true;
                        LCA = cur;
                    }
                }
                cur = cur.left;
            } else{
                TreeNode top = stack.peek();
                if(top.right != null && pre != top.right){
                    cur = top.right;
                }
                else{
                    pre = top;
                    if(LCA == stack.pop() && one_node_found)
                        LCA = stack.peek();
                }
            }
        }
        return root;
    }
}
```



