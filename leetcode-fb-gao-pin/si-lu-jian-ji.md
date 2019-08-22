# 思路简记

## 301. Remove Invalid Parentheses

TODO

## 273. Integer to English Words

TODO

## 953. Verifying an Alien Dictionary

给你一个words list,和一个字母序，判断word list是否是sorted的。首先需要将字母序的string，转换成一个map&lt;character, integer&gt; , value 代表是字母的priority。然后比较adjacent words，如果前缀相同则 长度长者大。

```java
class Solution {
    public boolean isAlienSorted(String[] words, String order) {
        int[] map = new int[26];
        for(int i = 0; i < order.length(); i++){
            char c = order.charAt(i);
            map[c - 'a'] = i;
        }

        for(int i = 0; i < words.length - 1; i++){
            String word1 = words[i];
            String word2 = words[i + 1];
            int min = Math.min(word1.length(), word2.length());
            int j = 0;
            for(j = 0; j < min; j++){
                char c1 = word1.charAt(j);
                char c2 = word2.charAt(j);
                if(map[c1 - 'a'] > map[c2 - 'a']) return false;
                else if(map[c1 - 'a'] < map[c2 - 'a']) break;
            }
            if(j == min && word1.length() > word2.length()) return false;
        }
        return true;
    }
}
```

## 67. Add Binary

模拟题，对于长短不一的两个string的相加，建议使用两个指针，分别判断string的长度。还有stringbuilder直接append数字会把它转换成string

```java
class Solution {
    public String addBinary(String a, String b) {
        int al = a.length();
        int bl = b.length();
        int i = al - 1, j = bl - 1, carry = 0;
        StringBuilder sb = new StringBuilder();
        while(i >= 0 || j >= 0){
            int sum = carry;
            if(i >= 0) sum += a.charAt(i--) - '0';
            if(j >= 0) sum += b.charAt(j--) - '0';
            sb.append(sum % 2);
            carry = sum / 2;
        }
        if(carry != 0) sb.append(carry);
        return sb.reverse().toString();
    }
}
```

## 297. Serialize and Deserialize Binary Tree

{% page-ref page="../data-structure/binary-tree/serialize-and-deserialize-binary-tree.md" %}

## 253. Meeting Room II

## 973. K Closet Points to Origin

## 238. Product Array Except Self

暴力解O\(n^2\)，其次可以用product数组。两个数组left, right。分别表示product of all the numbers to the left and all the numbers to the right of the index。 把左边的product乘以右边的product即可。但是这样空间复杂度是O\(n\)。我们可以利用output先存left的信息，然后再在构建right数组的时候直接算出结果。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        // left array
        // [1, 2, 6, 24]
        // right array
        // [24,24,12,4]
        int n = nums.length;
        int[] res = new int[n];
        res[0] = 1; 
        for(int i = 1; i < n; i++){
            res[i] = res[i - 1] * nums[i - 1];
        }
        
        int right = 1;
        for(int i = n - 1; i >= 0; i--){
            res[i] *= right;
            right *= nums[i];
        }
        return res;
    }
}
```

## 560. Subarray Sum Equals K

{% page-ref page="../data-structure/map/subarray-sum-equals-k.md" %}

## 721.  Accounts Merge

本质上是把问题转换成一个graph，寻找联通量，并给其着色。build graph + dfs/bfs

```java
class Solution {
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        // each email is a node and the name of the owner is just the color of the node
        // we can build a graph based on these nodes.
        // by using dfs/bfs, we will find all the connected component and then color them aka assign them with a name.
        HashMap<String, HashSet<String>> graph = new HashMap<>();
        HashMap<String, String> color = new HashMap<>();
        for(List<String> account : accounts){
            String name = account.get(0);
            for(int i = 1; i < account.size(); i++){
                String cur = account.get(i);
                color.put(cur, name);
                if(!graph.containsKey(cur)) graph.put(cur, new HashSet<String>());
                if(i == 1) continue;
                String pre = account.get(i - 1);
                graph.get(cur).add(pre);
                graph.get(pre).add(cur);
            }
        }
        List<List<String>> res = new LinkedList<>();
        HashSet<String> visited = new HashSet<>();
        for(String node : graph.keySet()){
            if(visited.add(node)){
                List<String> list = new LinkedList<>();
                DFS(list, node, graph, visited);
                Collections.sort(list);
                list.add(0, color.get(node));
                res.add(list);
            }
        }
        return res;
    }

    public void DFS(List<String> list, String str, HashMap<String, HashSet<String>> graph, 
    HashSet<String> visited){
        list.add(str);
        for(String next : graph.get(str)){
            if(visited.add(next)){
                DFS(list, next, graph, visited);
            }
        }
    }
}
```

## 125. Valid Palindrome

注意两个点left一定要小于right，还有两个function

```java
Character.isLetterOrDigit(char c)
Character.toLowerCase(char c)
```

```java
class Solution {
    public boolean isPalindrome(String s) {
        if(s.equals("")) return true;
        int left = 0, right = s.length() - 1;
        while(left < right){
            while(left < right && !Character.isLetterOrDigit(s.charAt(left))) left++;
            while(left < right && !Character.isLetterOrDigit(s.charAt(right))) right--;
            if(Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))){
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

## 426.  Convert Binary Search Tree to Sorted Doubly Linked List

{% page-ref page="../data-structure/binary-tree/convert-binary-search-tree-to-sorted-doubly-linked-list.md" %}

## 1. Two Sum

给予我微信名以尊重

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            int complement = target - nums[i];
            if(map.containsKey(complement)){
                return new int[]{map.get(complement), i};
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```

## 158. Read N Characters Given Read4 II - Call multiple times

```text
TODO
```


