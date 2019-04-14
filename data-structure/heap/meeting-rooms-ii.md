---
description: 253. Meeting Rooms II
---

# Meeting Rooms II

Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` \(si &lt; ei\), find the minimum number of conference rooms required.

**Example 1:**

```text
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```

**Example 2:**

```text
Input: [[7,10],[2,4]]
Output: 1
```

最naive的方式就是去做simulation。我们可以按会议的开始时间进行排序，然后遍历会议，如果遍历到当前会议，就去看一下之前开始的会议有没有结束的，如果有就继续用它的room，没有就新建新的room。我们可以用一个hashmap来记录会议室号和会议结束的时间。每次就去找会议结束时间小于当前会议开始的时间就好了，这样的时间复杂度是O\(nk\)

```java
class Solution {
    public int minMeetingRooms(Interval[] intervals) {
        // corner case
        if(intervals.length == 0) return 0;
        Arrays.sort(intervals, (a, b) -> a.start - b.start);
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < intervals.length; i++){
            boolean flag = false;
            for(Integer key : map.keySet()){
                if(map.get(key) <= intervals[i].start){
                    map.put(key, intervals[i].end);
                    flag = true;
                    break;
                }
            }
            if(!flag){
                map.put(map.size() + 1, intervals[i].end);
            }
        }
        return map.size();
    }
}
```

为了优化我们的思路，主要考虑map的遍历问题，我们其实不需要找之前所有的会议室，而只需要找最早结束的会议室即可，那问题就转换成求之前最小的会议结束时间。这样我们可以想到用min-heap来解决。用PriorityQueue来存每个会议室的结束时间，当遍历到当前时间时，只需要拿堆顶的值与会议开始时间对比，若会议开始时间大于等于最早的会议室结束时间，我们就接着用这个会议室，并更新它的会议结束时间，否则就新开一个会议室。这样的时间复杂度就变成了O\(nlogk\)

```java
class Solution {
    public int minMeetingRooms(Interval[] intervals) {
        // corner case
        if(intervals.length == 0) return 0;
        // pq
        PriorityQueue<Integer> q = new PriorityQueue<>((a, b) -> a - b);
        // sort the array by the start time
        Arrays.sort(intervals, (a, b) -> a.start - b.start);
        q.offer(intervals[0].end);
        for(int i = 1; i < intervals.length; i++){
            Interval interval = intervals[i];
            if(interval.start >= q.peek()){
                q.poll();
            }
            q.offer(interval.end);
        }
        return q.size();
    }
}
```

另外一种思路就是用treemap来做，对于会议的开始时间，我们对其映射值加一，对于会议的结束时间，我们对其映射值减一。因为我们是根据时间来排序，按照从小到大的时间顺序，遍历时间线，把对应的值加起来，总有一个时刻，会议室的数目最多，这个就是我们要求的结果。

```java
// O(nlogn)
class Solution {
    public int minMeetingRooms(Interval[] intervals) {
        // corner case
        if(intervals.length == 0) return 0;
        TreeMap<Integer, Integer> map = new TreeMap<>();
        for(int i = 0; i < intervals.length; i++){
            map.put(intervals[i].start, map.getOrDefault(intervals[i].start, 0) + 1);
            map.put(intervals[i].end, map.getOrDefault(intervals[i].end, 0) - 1);
        }
        int max = 0, values = 0;
        // The collection's iterator returns the values in ascending order of the corresponding keys
        for(Integer v : map.values()){
            values += v;
            max = Math.max(max, values);
        }
        return max;
    }
}
```



