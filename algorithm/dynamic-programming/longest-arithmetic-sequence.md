---
description: 'Leetcode 1027 https://leetcode.com/problems/longest-arithmetic-sequence/'
---

# Longest Arithmetic Sequence

Given an array `A` of integers, return the **length** of the longest arithmetic subsequence in `A`.

Recall that a _subsequence_ of `A` is a list `A[i_1], A[i_2], ..., A[i_k]` with `0 <= i_1 < i_2 < ... < i_k <= A.length - 1`, and that a sequence `B` is _arithmetic_ if `B[i+1] - B[i]` are all the same value \(for `0 <= i < B.length - 1`\).

**Example 1:**

```text
Input: [3,6,9,12]
Output: 4
Explanation: 
The whole array is an arithmetic sequence with steps of length = 3.
```

**Example 2:**

```text
Input: [9,4,7,2,10]
Output: 3
Explanation: 
The longest arithmetic subsequence is [4,7,10].
```

**Example 3:**

```text
Input: [20,1,15,3,10,5,8]
Output: 4
Explanation: 
The longest arithmetic subsequence is [20,15,10,5].
```

**Note:**

1. `2 <= A.length <= 2000`
2. `0 <= A[i] <= 10000`

## 思路

1. 暴力解是2^n
2. 这道题让人联想到[Longest Increasing Subsequence](longest-increasing-subsequence-todo.md)，可以用同样到DP思想做，因为等差可能是负数，所以没法用二维数组，只能改用Hashmap。HashMap&lt;Integer, HashMap&lt;Integer, Integer&gt;&gt;,这个等于dp\[i\]\[diff\]= length\(e.g. 2\)。我一开始用的是\[diff\]\[i\]超时。

```java
class Solution {
    public int longestArithSeqLength(int[] A) {
        if(A.length <= 2) return A.length;
        // length of the different
        HashMap<Integer, HashMap<Integer, Integer>> map = new HashMap<>();
        int max = 2;
        HashMap<Integer, Integer> s = new HashMap<>();
        s.put(A[0], 1);
        map.put(0, s);
        for(int i = 1; i < A.length; i++){
            HashMap<Integer, Integer> cur = new HashMap<>();
            for(int j = 0; j < i; j++){
                int diff = A[i] - A[j];
                cur.put(diff, 2);
                HashMap<Integer, Integer> pre = map.get(j);
                if (pre.containsKey(diff)) {
                    cur.put(diff, pre.get(diff) + 1);
                }
                max = Math.max(cur.get(diff), max);
            }
            map.put(i, cur);
        }
        return max;
    }
}
```



