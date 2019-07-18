# 思路简记

## 35. 翻转链表

* Iterative：定义两个指针cur, pre，一遍过
* Recursion： 对于当前node，先翻转后面的list，然后再把当前的node加进去即可

## 41. 最大子数组

```java
int cur = 0;
int max = Integer.MIN_VALUE;
for(int i = 0; i < nums.length; i++){
    cur = cur < 0 ? nums[i] : nums[i] + cur;
    max = Math.max(max, cur);
}
return max;
```

注意subarray至少有一个元素，这就意味着如果之前的sum小于0，也就没有必要再继续保留了，直接用当前元素替代即可。

## 1350. Excel表列标题

```java
public String convertToTitle(int n) {
        // write your code here
        StringBuilder sb = new StringBuilder();
        while(n != 0){
            sb.append((char)((n - 1) % 26 + 'A'));
            n = (n - 1) / 26;
        }
        return sb.reverse().toString();
}
```

这道题的Corner case 是26，对26取余时N应该减一。

## 167. 链表求和

```java
public ListNode addLists(ListNode l1, ListNode l2) {
        // write your code here
        int carry = 0, n1 = 0, n2 = 0;
        ListNode head = new ListNode(0);
        ListNode tmp = head;
        while(l1 != null || l2 != null || carry != 0){
            if(l1 == null) n1 = 0;
            else{
                n1 = l1.val;
                l1 = l1.next;
            }
            if(l2 == null) n2 = 0;
            else{
                n2 = l2.val;
                l2 = l2.next;
            }
            carry = n1 + n2 + carry;
            head.next = new ListNode(carry % 10);
            carry = carry / 10;
            head = head.next;
        }
        return tmp.next;
}
```

注意循环的条件，三个缺一不可，容易漏掉carry != 0

## 134. LRU

[LRU Cache](https://app.gitbook.com/@herots/s/algorithm/~/edit/drafts/-LjKqe-wDGlLRg0wa1mI/data-structure/map/lru-cache)

## 200. 最长回文子串

{% embed url="https://super468.github.io/2019/02/26/LongestPalindromeSubstring/\#more" %}

此题需要反复复习！！

## 423. 有效的括号序列

```java
public boolean isValidParentheses(String s) {
        // write your code here
        HashMap<Character, Character> map = new HashMap<>();
        map.put(')', '(');
        map.put(']', '[');
        map.put('}', '{');
        Stack<Character> stack = new Stack<>();
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(map.containsKey(c)){
                if(stack.isEmpty()) return false;
                char top = stack.pop();
                if(top != map.get(c)) return false;
            } else{
                stack.push(c);
            }
        }
        return stack.isEmpty();
}
```

用stack来存储opening bracket（括号）。当遇到对应closing bracket时取出。

## 419. 罗马数字转整数

```java
public int romanToInt(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);
        int pre = 0, sum = 0;
        for(int i = s.length() - 1; i >= 0; i--){
            char c = s.charAt(i);
            int num = map.get(c);
            sum += (pre > num ? -num : num);
            pre = num;
        }
        return sum;
}
```

从后向前遍历，如果当前的数比之前的小则减去否则加上。

## 149. 买卖股票的最佳时机

```java
public int maxProfit(int[] prices) {
        // write your code here
        int max = 0;
        int buy = prices[0];
        for(int i = 1; i < prices.length; i++){
            if(prices[i] < buy) buy = prices[i];
            else max = Math.max(max, prices[i] - buy);
        }
        return max;
}
```

思路还是贪心为主，不断找局部最优解，从而找到全局最优解。贪心思路在于低价买入，如果当前的价格比买入的价格低，卖掉是亏本买卖，那么当前价格就是新的买入价格。

## 104. 合并K个排序链表

```java
public ListNode mergeKLists(ListNode[] lists) {
    if(lists.length < 1) return null;
    return partion(lists, 0, lists.length - 1);
}

public ListNode partion(ListNode[] lists, int left, int right){
    if(left == right) return lists[left];
    if(left < right){
        int mid = (left + right) / 2;
        ListNode l_node = partion(lists, left, mid);
        ListNode r_node = partion(lists, mid + 1, right);
        return mergeList(l_node, r_node);
    }
    else return null;
    
}

public ListNode mergeList(ListNode list1, ListNode list2){
    if(list1 == null) return list2;
    if(list2 == null) return list1;
    if(list1.val < list2.val){
        list1.next = mergeList(list1.next, list2);
        return list1;
    }
    else{
        list2.next = mergeList(list1, list2.next);
        return list2;
    }
}
```

这道题有好几种解法

1. 暴力：将所有的节点存到数组中，排序数组，数组变链表
2. 每次比K个节点，选最小的节点加进去
3. 优化2，建立一个K大小的PriorityQueue
4. 一个一个merge
5. 分治法：将链表分成两部分，merge完之后，再将这两部分merge

3和5是O\(nlogK\)， 5难写一点，上面是5的代码，主要集中在mergeList 要写的简洁，partition用recursive简洁一点

**Update 1: 这个mergeTwoList recursive的写法不要忘记每次往后挪head**

## 100. 删除排序数组中的重复数字

```java
public int removeDuplicates(int[] nums) {
    int length = nums.length;
    if(length < 2) return length;
    int left = 0;
    for(int right = 0; right < length; right++){
        if(nums[left] != nums[right]){
            left++;
            nums[left] = nums[right];
        }
    }
    return left + 1;
}
```

这道题用两个指针，left代表目前所有的不重复的数字，right则寻找下一个不与left重复的数，注意最后返回的是left+1

## 64. 合并排序数组

```java
public void mergeSortedArray(int[] A, int m, int[] B, int n) {
    // write your code here
    int p1 = m - 1, p2 = n - 1, p = m + n - 1;
    while((p1 >= 0) && (p2 >= 0)){
        A[p--] = A[p1] > B[p2] ? A[p1--] : B[p2--];
    }
    System.arraycopy(B, 0, A, 0, p2 + 1);
}
```

A数组有足够的空间，也就是说后面是空白的。如果用两个指针从前往后merge的话，就需要额外的空间复制A数组。但是如果从后往前则可以利用A的空白部分。

Following is the declaration for **java.lang.System.arraycopy\(\)** method

```java
public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
```

* **src** − This is the source array.
* **srcPos** − This is the starting position in the source array.
* **dest** − This is the destination array.
* **destPos** − This is the starting position in the destination data.
* **length** − This is the number of array elements to be copied.

## 62. 搜索旋转排序数组

[Search in Rotated Sorted Array](https://app.gitbook.com/@herots/s/algorithm/~/edit/drafts/-Lj_tURzhrNuHqe7wW3k/algorithm/binary-search/search-in-rotated-sorted-array)

## 415. 有效回文串

```java
public boolean isPalindrome(String s) {
    // write your code here
    int left = 0, right = s.length() - 1;
    while(left < right){
        while(left < right && !Character.isLetterOrDigit(s.charAt(left))) left++;
        while(left < right && !Character.isLetterOrDigit(s.charAt(right))) right--;
        if(Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) return false;
        left++;
        right--;
    }
    return true;
}
```

此题易错,注意left和right的设置,这里终结条件是left == right, 则肯定是回文的。注意不是left &lt;= right

## 512. 解码方法

```java
public int numDecodings(String s) {
    int length = s.length();
    if(length < 1) return 0;
    int[] dp = new int[length + 1];
    dp[0] = 1;
    dp[1] = (s.charAt(0) != '0') ? 1 : 0;
    for(int i = 2; i <= s.length(); i++){
        int num1 = Integer.valueOf(s.substring(i - 1, i));
        int num2 = Integer.valueOf(s.substring(i - 2, i));
        if(num1 >= 1 && num1 <= 9) dp[i] += dp[i - 1];
        if(num2 >= 10 && num2 <= 26) dp[i] += dp[i - 2];
    }
    return dp[length];
}
```

此题使用一维DP，一定要注意0的情况，比如在首位以及10，20等，所以需要使用valueOf

## 433. 岛屿数目

```java
public int numIslands(boolean[][] grid) {
    // write your code here
    for(int i = 0; i < grid.length; i++){
        for(int j = 0; j < grid[0].length; j++){
            if(grid[i][j] == true){
                count++;
                DFS(grid, i, j);
            }
        }
    }
    return count;
}
    
public void DFS(boolean[][] grid, int i, int j){
    if(i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == false) return;
    grid[i][j] = false;
    DFS(grid, i + 1, j);
    DFS(grid, i - 1, j);
    DFS(grid, i, j + 1);
    DFS(grid, i, j - 1);
}
```

DFS，BFS，Union Find 三种做法，DFS直接秒

## 102. 二叉树的层次遍历

**DFS**

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    //preorder 3, 9 20, 15, 7
    //inorder 9, 3, 15, 20, 7
    //postorder 9, 15, 7, 20, 3
    List<List<Integer>> res = new ArrayList<>();
    preOrder(root, 1, res);
    return res;
}
    
public void preOrder(TreeNode node, int depth, List<List<Integer>> res){
    if(node == null) return;
    if(depth > res.size()){
        res.add(new ArrayList<>());
    }
    res.get(depth - 1).add(node.val);
    
    preOrder(node.left, depth + 1, res);
    preOrder(node.right, depth + 1, res);
}
```

BFS

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if(root == null) return res;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while(!queue.isEmpty()){
        List<Integer> list = new ArrayList<>();
        for(int k = queue.size() - 1; k >= 0; k--){
            TreeNode top = queue.poll();
            list.add(top.val);
            if(top.left != null) queue.offer(top.left);
            if(top.right != null) queue.offer(top.right);
        }
        res.add(list);
    }
    return res;
}
```

## 95. 验证二叉树

**Recursion**

```java
public boolean isValidBST(TreeNode root) {
    return isValid(root, null, null);
}

public boolean isValid(TreeNode node, Integer low, Integer high){
    if(node == null) return true;
    int val = node.val
    if(low != null && node.val <= low) return false;
    if(high != null && node.val >= high) return false;
    
    return isValid(node.left, low, node.val) && isValid(node.right, node.val, high);
}
```

**Inorder Traversal**

```java
public boolean isValidBST(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    if(root == null) return true;
    double pre = - Double.MAX_VALUE;
    TreeNode cur = root;
    while(cur != null || !stack.isEmpty()){
        if(cur != null){
            stack.push(cur);
            cur = cur.left;
        } else{
            TreeNode top = stack.pop();
            if(pre >= top.val) return false;
            pre = top.val;
            if(top.right != null)
                cur = top.right;
        }
    }
    return true;
}
```

Recursion的方法让我一阵好想，给两个子树一个范围确实是一个妙招

## 421. 简化路径

```java
public String simplifyPath(String path) {
    Stack<String> stack = new Stack<>();
    String[] strs = path.split("/");
    Set<String> skip = new HashSet<>(Arrays.asList("..",".",""));
    for(String str : strs){
        if(str.equals("..") && !stack.isEmpty()) stack.pop();
        else if(!skip.contains(str)) stack.push(str);
    }
    String s = "";
    while(!stack.isEmpty()){
        s = "/" + stack.pop() + s;
    }
    return s.equals("") ? "/" : s;
}
```

注意path split的时候，会有可能split出空字符串“”

## 88. 最近公共祖先

```java
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
```

[Lowest Common Ancestor of a Binary Tree](https://app.gitbook.com/@herots/s/algorithm/~/edit/drafts/-LjprX8IwFnofjlNvHvu/data-structure/binary-tree/lowest-common-ancestor-of-a-binary-tree)

