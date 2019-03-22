---
description: postorder是left-right-mid的状态，非递归版本比较难一直没写过，这篇写个postorder的Iteration版本
---

# Postorder Traversal

145. Binary Tree Postorder TraversalHard79040FavoriteShare

Given a binary tree, return the _postorder_ traversal of its nodes' values.

**Example:**

```text
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [3,2,1]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

思路：

* 将每个节点都开成父节点，包括叶子节点（其包含两个空的子节点）
* 找到当前节点的最左节点，如果它有右节点就先访问右节点，最后访问父节点。
* 我们需要一个last节点来记录上次访问的节点，如果是左节点，就访问右节点，如果是右节点，就访问父节点。这样可以保证只访问一次右节点。

代码1：

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        List<Integer> list = new ArrayList<>();
        TreeNode cur = root;
        TreeNode last = null;
        while(cur != null || !stack.empty()){
            if(cur != null){
                stack.push(cur);
                cur = cur.left;
            } else{
                TreeNode top = stack.peek();
                if(top.right != null && last != top.right){
                    cur = top.right;
                } else{
                    list.add(top.val);
                    last = top;
                    stack.pop();
                }
            }
        }
        return list;
    }
}
```

代码2：

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        List<Integer> list = new ArrayList<>();
        TreeNode pre = null;
        if(root == null) return list;
        stack.push(root);
        while(!stack.empty()){
            TreeNode top = stack.peek();
            if((top.left == null && top.right == null) ||
               (pre != null && (pre == top.right || pre == top.left))){
                list.add(top.val);
                pre = top;
                stack.pop();
            } else{
                if(top.right != null) stack.push(top.right);
                if(top.left != null) stack.push(top.left);
            }
        }
        return list;
    }
}
```

[这篇博客写的不错](https://blog.csdn.net/zhangxiangDavaid/article/details/37115355)

