---
description: Leetcode 159
---

# Longest Substring with At Most Two Distinct Characters

使用双指针解决字符串问题：

先做字符串右侧指针的增加；

进行条件判断，这里比较特殊就是不是一满足2个就进行计算，这样的话得到的字符串不是最长的，如aabbc，在第一个b的地方要接着向前走。所以判断为3个；

每次fast指针位移一次，保证了上一次的更新被存在了结果里面（13行）；也就是aabb的长度。



```java
    
class Solution { 
public int lengthOfLongestSubstringTwoDistinct(String s) { 
    if(s.length() <= 2) return s.length();
    int[] chars = new int[256];
    int res = 0;

    for(int slow = 0, fast = 0, diff = 0; fast < s.length(); ){
        if(chars[s.charAt(fast++)]++ == 0) diff++;

        while(diff == 3 && if(--chars[s.charAt(slow++)] == 0)) diff--;
        
        res = Math.max(res, fast - slow);
    }
    return res;

}
```

}

