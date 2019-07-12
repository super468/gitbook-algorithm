---
description: LeetCode 33
---

# Search in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

\(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`\).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of _O_\(log _n_\).

**Example 1:**

```text
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```text
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

思路：这道二分经典题，需要分情况讨论，Corner case多，给出标准写法，多多复习

```java
public int search(int[] nums, int target) {
    int left = 0,  right = nums.length - 1;
    while(left <= right){ // 注意 <=
        int mid = left + (right - left) / 2;
        if(target == nums[mid]){
            return mid;
        } // 注意这里将nums[left]做为判断的标准，在讨论左边的时候需要吧left带上, 也就是nums[mid] == nums[left] 
        else if(nums[mid] < nums[left]){   
            if(target > nums[mid] && target <= nums[right]){
                left = mid + 1;  // 注意
            } else{
                right = mid - 1;
            }
        } else{
            if(target < nums[mid] && target >= nums[left]){
                right = mid - 1;
            } else{
                left = mid + 1;
            }
        }
    }
    return -1;
}
```

