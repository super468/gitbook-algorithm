---
description: Leetcode 4
---

# Median of Two Sorted Arrays\(TODO\)

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O\(log \(m+n\)\).

You may assume **nums1** and **nums2** cannot be both empty.

**Example 1:**

```text
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```text
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

## 思路

暴力解将两个数组合并，找到中值，时间复杂度为O\(N\)

利用sorted的特性将问题转化为如何求两个排序序列的第K小的数。我们可以先找到两个数组的第K/2个数，如果A数组的A\[k/2\] &lt; B\[k/2\] 那么包括A\[k/2\]在内的前面的所有数都不可能是第K个数。A\[k/2\]最大的可能性是第K-1个数。所以我们删除这k/2个数，然后将问题转换为求剩下数组中的第k- k/2个数。其中的一个Corner case是有可能k/2大于数组的长度，那么就只能拿数组的最后一位对比了。结束的条件是一个数组为空或者k == 1。因为每次减去k/2，所以时间复杂度是log\(k\)，k = \(m + n\) / 2，所以最后的时间复杂度是O\(log\(m + n\)\)。空间复杂度因为是尾递归，编译器优化堆栈，所以复杂度为O\(1\)，否则也是log\(m + n\)

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        // find the kth in two sorted array
        int m = nums1.length;
        int n = nums2.length;
        int left = (m + n + 1) / 2;
        int right = (m + n + 2) / 2;
        return (findKthSortedArrays(nums1, 0, nums2, 0, left) + 
        findKthSortedArrays(nums1, 0, nums2, 0, right)) * 0.5;
    }

    public double findKthSortedArrays(int[] nums1, int start1, int[] nums2, int start2, int k){
        int len1 = nums1.length - start1;
        int len2 = nums2.length - start2;
        if(len1 > len2) return findKthSortedArrays(nums2, start2, nums1, start1, k);
        if(len1 == 0) return nums2[start2 + k - 1];
        if(k == 1) return Math.min(nums1[start1], nums2[start2]);

        int mid1 = start1 + Math.min(len1, k / 2) - 1;
        int mid2 = start2 + Math.min(len2, k / 2) - 1;
        if(nums1[mid1] > nums2[mid2]){
            return findKthSortedArrays(nums1, start1, nums2, mid2 + 1, k - (mid2 - start2 + 1));
        } else{
            return findKthSortedArrays(nums1, mid1 + 1, nums2, start2, k - (mid1 - start1 + 1));
        }
    }
}
```

log\(min\(m + n\)\) [https://zhuanlan.zhihu.com/p/55666669](https://zhuanlan.zhihu.com/p/55666669)



