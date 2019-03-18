---
description: 非递归版本 && Morris Traversal
---

# Preorder Traversal

144. Binary Tree Preorder Traversal

Given a binary tree, return the _preorder_ traversal of its nodes' values.

**Example:**

```text
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

非递归版本：

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        List<Integer> list = new ArrayList<>();
        if(root == null) return list;
        stack.push(root);
        while(!stack.empty()){
            TreeNode top = stack.pop();
            list.add(top.val);
            if(top.right != null) stack.push(top.right);
            if(top.left != null) stack.push(top.left);
        }
        return list;
    }
}
```

* [ ] Morris:

