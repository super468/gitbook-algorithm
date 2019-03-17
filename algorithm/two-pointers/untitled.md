# 3Sum

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

Example:

```text
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

解法： 3Sum 标准解法 two pointer, 先给数组排序，然后将a + b + c = 0的问题，转换成a + b = -c的问题，-c即为target。 1. 因为答案的要求是不能出现重复的解，所以需要先去除重复的target。在走第一层循环的时候，要过滤掉重复出现的数字。 2. 关于指针从哪里开始的问题，因为是不能有重复解，所以左指针应该从基于当前的target，还没访问的最小的值开始，右指针从最大值开始。 3. 当寻找的解后，应该避免duplicate，将左右指针分别移至与其当前值不相同

代码：

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        for(int i = 0; i < nums.length - 2; i++){
            //skip the duplicate target
            if(i != 0 && nums[i] == nums[i - 1]) continue;
            int j = i + 1, k = nums.length - 1, target = -nums[i];
            while(j < k){
                int sum = nums[j] + nums[k];
                if(target > sum){
                    j++;
                } else if(target < sum){
                    k--;
                } else {
                    result.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    //move the pointer to avoid duplicates
                    //pointers must move at least one step
                    while(j < k && nums[j] == nums[++j]);
                    while(k > i && nums[k] == nums[--k]);
                }
            }
        }
        return result;
    }
}
```

