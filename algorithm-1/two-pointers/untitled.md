# 3Sum

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

Example:

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```
解法：

代码：
```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        for(int i = 0; i < nums.length - 2; i++){
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
                    while(j < k && nums[j] == nums[++j]);
                    while(k > i && nums[k] == nums[--k]);
                }
            }
        }
        return result;
    }
}
```
fefef