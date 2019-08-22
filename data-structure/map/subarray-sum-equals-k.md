---
description: Leetcode 560
---

# Subarray Sum Equals K

Given an array of integers and an integer **k**, you need to find the total number of continuous subarrays whose sum equals to **k**.

**Example 1:**  


```text
Input:nums = [1,1,1], k = 2
Output: 2
```

**Note:**  


1. The length of the array is in range \[1, 20,000\].
2. The range of numbers in the array is \[-1000, 1000\] and the range of the integer **k** is \[-1e7, 1e7\].

## 思路

暴力解O\(n^3\)，其次我们使用cumulative sum。sum of subarray\[i,j\] = sum\[j\] -sum\[i\] 时间复杂度是O\(n^2\)  
除此之外解决substring，subarray最common的暴力解是在寻找每一个substring的过程中去判断一下满足的情况，对于特定的start pointer，我们iterate over all the possible end pointers。sum 加入 end对应的元素后如果等于K，那么count++。当start + 1，sum应该清空为0

对于这种subarray的题目，可能会想到用sliding window。但是sliding window只能解决满足特定条件的问题：

1. 如果宽的sliding window是valid，那么窄的sliding window也要满足valid
2. 如果窄的sliding window是invalid，那么宽的sliding window也要满足invalid

具体请见

{% page-ref page="../../algorithm/two-pointers/sliding-window/" %}

还有一种O\(n\)时间复杂度的方法，就是prefix sum + map。中心思想就是如果对于index j, cumulative sum to j is sum。如果我们能够在j之前有过 cumulative sum to i 等于 sum - k, 那么 sum of subarray\[i, j\] equals to k。我们需要利用hashmap记录occurences of sum。当上述情况发生时，我们知道sum 等于 sum - k的情况出现了多少次，count直接加上这个数就可以了

## 代码

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0, sum = 0;
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        for(int i = 0; i < nums.length; i++){
            sum += nums[i];
            if(map.containsKey(sum - k)){
                count += map.get(sum - k);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
```





