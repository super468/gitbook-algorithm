# Path Sum

112. Path Sum

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

**Note:** A leaf is a node with no children.

**Example:**

Given the below binary tree and `sum = 22`,

```text
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```

return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.

思路：

这道题是面试微软时遇到的，首先递归的写法很简单，可以把它看成DFS的递归版本。那么怎么写DFS的非递归版本呢，BFS呢？事实上，树的遍历本是可以看成一种特殊的DFS，前序，中序，后序遍历都是特殊的DFS。递归的写法可以看成是前序遍历的递归实现。那问题就转化成怎么用前序遍历的非递归版本？我们可以维护两个栈，一个用来存节点遍历，一个用来存该节点的sum。但是这样就多需要维护一个数字栈。如果我们用后序遍历就可以将父节点的值传给左节点用完之后再给右节点用。

BFS：

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        Queue<TreeNode> q1 = new LinkedList<>();
        Queue<Integer> q2 = new LinkedList<>();
        if(root == null) return false;
        q1.offer(root);
        q2.offer(root.val);
        while(!q1.isEmpty()){
            TreeNode top = q1.poll();
            int top_val = q2.poll();
            if(top.left == null && top.right == null){
                if(top_val == sum) return true;
            }
            if(top.left != null){
                int tmp = top_val + top.left.val;
                q1.offer(top.left);
                q2.offer(tmp);
            }
            if(top.right != null){
                int tmp = top_val + top.right.val;
                q1.offer(top.right);
                q2.offer(tmp);
            }
        }
        return false;
    }
}
```

前序遍历：

```java
class Solution {
  public boolean hasPathSum(TreeNode root, int sum) {
    if (root == null)
      return false;

    LinkedList<TreeNode> node_stack = new LinkedList();
    LinkedList<Integer> sum_stack = new LinkedList();
    node_stack.add(root);
    sum_stack.add(sum - root.val);

    TreeNode node;
    int curr_sum;
    while ( !node_stack.isEmpty() ) {
      node = node_stack.pollLast();
      curr_sum = sum_stack.pollLast();
      if ((node.right == null) && (node.left == null) && (curr_sum == 0))
        return true;

      if (node.right != null) {
        node_stack.add(node.right);
        sum_stack.add(curr_sum - node.right.val);
      }
      if (node.left != null) {
        node_stack.add(node.left);
        sum_stack.add(curr_sum - node.left.val);
      }
    }
    return false;
  }
}
```

后序遍历1：

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode cur = root;
        TreeNode pre = null;
        int target = 0;
        while(cur != null || !stack.empty()){
            if(cur != null){
                target += cur.val;
                stack.push(cur);
                cur = cur.left;
            } else {
                TreeNode top = stack.peek();
                if(top.right == null && top.left == null){
                    if(target == sum) return true;
                }
                if(top.right != null && pre != top.right){
                    cur = top.right;
                } else{
                    target -= top.val;
                    pre = top;
                    stack.pop();
                }
            }
        }
        return false;
    }
}
```

后序遍历2：

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        Stack<TreeNode> stack = new Stack<>();
        if(root == null) return false;
        stack.push(root);
        TreeNode pre = null;
        int target = 0;
        while(!stack.empty()){
            TreeNode top = stack.peek();
            //第一次访问
            if(pre == null || (pre != top.left && pre != top.right)){
                target += top.val;
                if(top.right != null) stack.push(top.right);
                if(top.left != null) stack.push(top.left);
            } else {
                pre = top;
                stack.pop();
                target -= top.val;
            }
            if(top.left == null && top.right == null){
                if(target == sum) return true;
                pre = top;
                stack.pop();
                target -= top.val;
            }
        }
        return false;
    }
}
```

这里while循环内部，分几种情况，也是遍历的时候会遇到的：

1. 第一次访问（根节点，没有上次出栈节点）或者上次出栈的节点不是当前节点的子女节点（是兄妹关系）；
2. 上次出栈的节点和当前节点是父子关系；
3. （不管case1还是case2）当前节点都可能是叶子节点；

所以，先判断关系，如果是兄妹关系，那么本分支就是一个全新的分支，需要入栈子节点（本节点暂不出栈，同时子女要入栈）；如果是子女，那就说明左右子节点都访问完毕了，可以计算或者打印这个节点了（出栈）；最后，无论是哪种情况，如果这个是叶子节点的话（出栈），并进行相关的计算。

总体来说，只要无儿无女，就免不了要被扫地出门了：）







