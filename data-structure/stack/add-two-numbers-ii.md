# Add Two Numbers II

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Follow up:**  
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

**Example:**

```text
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

解法：

* 比较暴力的解法就是reverse list 然后两个list相加，如果需要不改变list的话，可以利用两个stack分别存list，stack的特性让我们始终都是在处理最小位的数。
* 代码里需要注意的是，当merge 两个不同长度的list时，应该将while循环写成 \|\| 而不是 &&，这样就可以在循环里面处理多出来的部分。

代码：

```java
class Solution {
    // 10 base carry is 1 at most
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Stack<ListNode> s1 = new Stack<>();
        Stack<ListNode> s2 = new Stack<>();
        while(l1 != null){
            s1.push(l1);
            l1 = l1.next;
        }
        while(l2 != null){
            s2.push(l2);
            l2 = l2.next; 
        }
        ListNode head = null;
        int sum = 0;
        // use || rather than && to write clean code
        while(!s1.empty() || !s2.empty()){
            if(!s1.empty()) sum += s1.pop().val;
            if(!s2.empty()) sum += s2.pop().val;
            
            ListNode tmp = new ListNode(sum % 10);
            tmp.next = head;
            head = tmp;
            
            sum /= 10;
        }
        
        // handle 0 is the first
        if(sum == 1){
            ListNode tmp = new ListNode(1);
            tmp.next = head;
            return tmp;
        }
        
        return head;
    }
}
```



