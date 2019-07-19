---
description: LeetCode 75
---

# Sort Colors

Given an array with _n_ objects colored red, white or blue, sort them [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) ****so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

**Note:** You are not suppose to use the library's sort function for this problem.

**Example:**

```text
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

**Follow up:**

* A rather straight forward solution is a two-pass algorithm using counting sort.  First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
* Could you come up with a one-pass algorithm using only constant space?

## 思路：

这道题首先可以用counting sort做，是一个two pass 。如果要是one pass的话，考虑使用双指针来做，给数字0，2各设置一个指针，0指针前的数全是0，2指针后的数全是2，这样剩下的就都是1了。这种做法的tricky的地方在于当你把2交换到后面去的时候也许并没有结束，此时不应该move on到下一个元素。因为你交换过来的元素有可能是0，需要再次进行交换到左边。为什么0交换的时候没有这个问题呢，元素是从左向右遍历的，当你遍历到这个元素是已经说明左边已经clear了没有2了。下面有两种写法：

```java
void sortColors(int A[], int n) {
    int j = 0, k = n-1;
    for (int i=0; i <= k; i++) {
        if (A[i] == 0)
            swap(A[i], A[j++]);
        else if (A[i] == 2)
            swap(A[i--], A[k--]);
    }
}
```

```java
void sortColors(int A[], int n) {
    int j = 0, k = n - 1;
    for (int i = 0; i <= k; ++i){
        if (A[i] == 0 && i != j)
            swap(A[i--], A[j++]);
        else if (A[i] == 2 && i != k)
            swap(A[i--], A[k--]);
    }
}
```

这种方式的one pass其实不如two pass counting sort，但是它最多有O\(2N\)，每个元素最多被操作两次。还有一种更牛逼的方法，就是one pass同时更新0，1，2的末尾，用前数覆盖后数的方法。

```java
void sortColors(int A[], int n) {
    int n0 = -1, n1 = -1, n2 = -1;
    for (int i = 0; i < n; ++i) {
        if (A[i] == 0) 
        {
            A[++n2] = 2; A[++n1] = 1; A[++n0] = 0;
        }
        else if (A[i] == 1) 
        {
            A[++n2] = 2; A[++n1] = 1;
        }
        else if (A[i] == 2) 
        {
            A[++n2] = 2;
        }
    }
}
```

