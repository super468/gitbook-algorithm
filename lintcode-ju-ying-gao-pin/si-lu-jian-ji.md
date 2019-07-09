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

## Have you had a chance to answer the previous question?

Yes, after a few months we finally found the answer. Sadly, Mike is on vacations right now so I'm afraid we are not able to provide the answer at this point.

