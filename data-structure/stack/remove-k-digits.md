# Remove K Digits

Given a non-negative integer num represented as a string, remove kdigits from the number so that the new number is the smallest possible.

**Note:**

* The length of num is less than 10002 and will be ≥ k.
* The given num does not contain any leading zero.

**Example 1:**

```text
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
```

**Example 2:**

```text
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
```

**Example 3:**

```text
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
```

解法：

拿到这道题，首先应该想的是，如果去除一个的话，怎么去除？应该是从左向右，找到比其右邻居大的一个元素，将其删除，如果删两个的话，就是在这个基础上重复上述的操作，这样，我们就需要遍历k次string，时间复杂度为O\(NK\)。我们能不能保存删除节点之前的信息呢？利用stack来存之前遍历过的元素即可！！

代码：

```java
class Solution {
    public String removeKdigits(String num, int k) {
        StringBuilder sb = new StringBuilder();
        int length = num.length();
        if(k == length) return "0";
        Stack<Character> stack = new Stack<>();
        for(int i = 0; i < length; i++){
            char c = num.charAt(i);
            while(k > 0 && !stack.empty() && c < stack.peek()){
                stack.pop();
                k--;
            }
            stack.push(c);
        }
        while(k > 0){
            stack.pop();
            k--;
        }
        while(!stack.empty()){
            sb.append(stack.pop());
        }
        sb = sb.reverse();
        while(sb.length() > 1 && sb.charAt(0) == '0'){
            sb.deleteCharAt(0);
        }
        return sb.toString();
    }
}
```



