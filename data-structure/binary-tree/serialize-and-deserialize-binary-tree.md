---
description: Leetcode 297
---

# Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Example:** 

```text
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
```

**Clarification:** The above format is the same as [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Note:** Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

## 思路

关于Binary Tree总是绕不过DFS和BFS，序列化BT时，需要加separator，这样在反序列化的时候便于把节点split出来。

## 复杂度

空间：O\(n\)  
时间：O\(n\)

## 代码

### DFS

```java
public class Codec {


    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        helper(root, sb);
        return sb.substring(0, sb.length() - 1);
    }

    public void helper(TreeNode node, StringBuilder sb){
        if(node == null) {
            sb.append("#").append(",");
        } else{
            sb.append(node.val).append(",");
            helper(node.left, sb);
            helper(node.right, sb);
        }
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] splits = data.split(",");
        Queue<String> queue = new LinkedList<>(Arrays.asList(splits));
        return helper2(queue);
    }

    public TreeNode helper2(Queue<String> queue){
        if(queue.isEmpty()) return null;
        String top = queue.poll();
        if(top.equals("#")) return null;
        TreeNode node = new TreeNode(Integer.valueOf(top));
        node.left = helper2(queue);
        node.right = helper2(queue);
        return node;
    }
}
```

### BFS

```java
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        StringBuilder sb = new StringBuilder();
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode top = queue.poll();
            if(top == null){
                sb.append("#").append(",");
            } else{
                sb.append(top.val).append(",");
                queue.offer(top.left);
                queue.offer(top.right);
            }
        }
        return sb.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        Queue<String> dataQueue = new LinkedList<>(Arrays.asList(data.split(",")));
        Queue<TreeNode> nodeQueue = new LinkedList<>();
        String first = dataQueue.poll();
        TreeNode root = null;
        if(first != null && !first.equals("#")){
            root = new TreeNode(Integer.valueOf(first));
            nodeQueue.offer(root);
        }
        while(!nodeQueue.isEmpty()){
            TreeNode topNode = nodeQueue.poll();
            String topData1 = dataQueue.isEmpty() ? "#" : dataQueue.poll();
            String topData2 = dataQueue.isEmpty() ? "#" : dataQueue.poll();
            if(!topData1.equals("#")){
                TreeNode left = new TreeNode(Integer.valueOf(topData1));
                topNode.left = left;
                nodeQueue.offer(left);
            }
            if(!topData2.equals("#")){
                TreeNode right = new TreeNode(Integer.valueOf(topData2));
                topNode.right = right;
                nodeQueue.offer(right);
            }
        }
        return root;
    }
}
```



