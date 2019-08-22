# Binary Search

二分查找有几种写法？它们的区别是什么？ - 胖君的回答 - 知乎 [https://www.zhihu.com/question/36132386/answer/155438728](https://www.zhihu.com/question/36132386/answer/155438728)

二分查找有几种写法？它们的区别是什么？ - LightGHLi的回答 - 知乎 [https://www.zhihu.com/question/36132386/answer/105595067](https://www.zhihu.com/question/36132386/answer/105595067)

## Lower Bound

对于不下降序列a，n为序列a元素的个数，key为关键字：

 1.求最小的i，使得a\[i\] = key，若不存在，则返回-1

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while(left < right){
            int mid = left + (right - left) / 2; //向下取整
            if(nums[mid] < target){
                left = mid + 1;
            } else{
                right = mid;
            }
        }
        if(nums[left] == target) return left;
        else return -1;
    }
}
```

## Higher Bound

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while(left < right){
            int mid = left + (right - left + 1) / 2; //向上取整
            if(nums[mid] <= target){
                left = mid;
            } else{
                right = mid - 1;
            }
        }
        if(nums[right] == target) return right;
        else return -1;
    }
}
```

