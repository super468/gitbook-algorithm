# Binary Tree Maximum Path Sum

124. Binary Tree Maximum Path Sum

Given a **non-empty** binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.

**Example 1:**

```text
Input: [1,2,3]

       1
      / \
     2   3

Output: 6
```

**Example 2:**

```text
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```

思路：

最长的路径可能在一颗子树，所以先判断子树里最长的路径，再决定是否将自己和子节点加入的父节点的最长路径中。有点像topological sort。递归版本的方法比较好写，非递归版本需要postorder，比较难写。需要再维护一个栈来保存子树的结果。

递归代码：

```java
class Solution {
    private int max;
    public int maxPathSum(TreeNode root) {
        max = Integer.MIN_VALUE;
        getMax(root);
        return max;
    }
    public int getMax(TreeNode root){
        if(root == null) return 0;
        int left = getMax(root.left);
        int right = getMax(root.right);
        max = Math.max(left + right + root.val, max);
        int res = Math.max(root.val, root.val + Math.max(left, right));
        return res > 0 ? res : 0; 
    }
}
```

非递归代码：

```java
class Solution {
    private int max;
    public int maxPathSum(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        Stack<Integer> helper = new Stack<>();
        if(root == null) return 0;
        max = Integer.MIN_VALUE;
        TreeNode cur = root;
        TreeNode last = root;
        while(cur != null ||!stack.empty()){
            if(cur != null){
                stack.push(cur);
                cur = cur.left;
            } else{
                TreeNode top = stack.peek();
                if(top.left == null && last != top.right){
                    helper.push(0);
                }
                if(top.right == null) helper.push(0);
                if(top.right != null && last != top.right){
                    cur = top.right;
                } else{
                    int right = helper.pop();
                    int left = helper.pop();
                    max = Math.max(left + right + top.val, max);
                    int tmp = top.val + Math.max(left, right);
                    helper.push(tmp > 0 ? tmp : 0);
                    last = top;
                    stack.pop();
                }
            }
        }
        return max;
    }
}
```



