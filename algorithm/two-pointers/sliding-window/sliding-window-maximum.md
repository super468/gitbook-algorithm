---
description: LeetCode 239
---

# Sliding Window Maximum

Given an array _nums_, there is a sliding window of size _k_ which is moving from the very left of the array to the very right. You can only see the _k_numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

**Example:**

```text
Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Note:**   
You may assume _k_ is always valid, 1 ≤ k ≤ input array's size for non-empty array.

**Follow up:**  
Could you solve it in linear time?

## 思路

naive way 是O\(NK\), 如果我们可以使用一种数据结构维护一个monotonic queue来存储最大值以及potential最大值就可以做到O\(N\)，这种数据结构就是Deque，可以在双端进行poll offer操作。

## 复杂度

时间复杂度：O\(N\)

空间复杂度：O\(N\)

## 代码

```java
class Solution {
    // bf
    //O((n - k + 1) * k)
    // monotonic queue
    // O(n - k + 1) 
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums.length < 1) return nums;
        int[] res = new int[nums.length + 1 - k];
        // store element
        Deque<Integer> queue = new ArrayDeque<>();
        for(int i = 0; i < nums.length; i++){
            // if nums[i] == peekLast, dont pollLast
            while(!queue.isEmpty() && queue.peekLast() < nums[i]){
                queue.pollLast();
            }
            queue.offerLast(nums[i]);
            if(i >= k - 1){
                res[i + 1 - k] = queue.peekFirst();
                if(queue.peekFirst() == nums[i + 1 - k]){
                    queue.pollFirst();
                }
            }
        }
        return res;
    }
}
```



