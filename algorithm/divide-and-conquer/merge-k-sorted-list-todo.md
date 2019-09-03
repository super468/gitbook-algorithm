---
description: 'Leetcode 23 https://leetcode.com/problems/merge-k-sorted-lists/'
---

# Merge K Sorted List\(TODO\)

Merge _k_ sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Example:**

```text
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

这道题有好几种解法

1. 暴力：将所有的节点存到数组中，排序数组，数组变链表
2. 每次比K个节点，选最小的节点加进去
3. 优化2，建立一个K大小的PriorityQueue
4. 一个一个merge
5. 分治法：将链表分成两部分，merge完之后，再将这两部分merge

3和5是O\(nlogK\)， 5难写一点，上面是5的代码，主要集中在mergeList 要写的简洁，partition用recursive简洁一点

**Update 1: 这个mergeTwoList iterative的写法不要忘记每次往后挪head**

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

