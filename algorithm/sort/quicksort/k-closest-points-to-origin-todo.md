---
description: LeetCode 973
---

# K Closest Points to Origin\(TODO\)

We have a list of `points` on the plane.  Find the `K` closest points to the origin `(0, 0)`.

\(Here, the distance between two points on a plane is the Euclidean distance.\)

You may return the answer in any order.  The answer is guaranteed to be unique \(except for the order that it is in.\)

**Example 1:**

```text
Input: points = [[1,3],[-2,2]], K = 1
Output: [[-2,2]]
Explanation: 
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest K = 1 points from the origin, so the answer is just [[-2,2]].
```

**Example 2:**

```text
Input: points = [[3,3],[5,-1],[-2,4]], K = 2
Output: [[3,3],[-2,4]]
(The answer [[-2,4],[3,3]] would also be accepted.)
```

**Note:**

1. `1 <= K <= points.length <= 10000`
2. `-10000 < points[i][0] < 10000`
3. `-10000 < points[i][1] < 10000`

## 思路

经典的Kth Problem。

1. 直接排序
2. Heap Sort
3. Quick Select

## 复杂度

1. 直接排序
2. Heap Sort
3. Quick Select 时间复杂度O\(N\) 
   1. 因为我们每次只处理一半的case，n + n / 2 + n / 4 + ... 最后是小于2n。但是worst case是O\(n^2\), 每个pivot都在Corner。

## 代码

Quick Select

```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        QuickSelect(points, 0, points.length - 1, K);
        return Arrays.copyOfRange(points, 0, K);
    }

    public void QuickSelect(int[][] points, int i, int j, int k){
        if(i > j) return;
        int pivot = Partition(points, i, j);
        if(pivot == k) return;
        else if(pivot > k){
            QuickSelect(points, i, pivot - 1, k);
        } else{
            QuickSelect(points, pivot + 1, j, k);
        }
    }

    public int Partition(int[][] points, int i, int j){
        int pivot = getDist(points[j]);
        int location = i;
        for(int start = i; start < j; start++){
            if(getDist(points[start]) < pivot){
                swap(points, start, location);
                location++;
            }
        }
        swap(points, j, location);
        return location;
    }

    public void swap(int[][] points, int i, int j){
        int[] tmp = points[i];
        points[i] = points[j];
        points[j] = tmp;
    }

    public int getDist(int[] point){
        return point[0] * point[0] + point[1] * point[1];
    }
}
```



