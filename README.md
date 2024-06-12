# LeetCode4-Median-Of-Two-Sorted-Arrays

Here we will discuss several ways to solve the following LeetCode [problem](https://leetcode.com/problems/median-of-two-sorted-arrays/description/) with Java.

If we omit the part that says that the complexity should be O(log (m+n)) we will have many ways of solving this problem so first we will explore a basic solution that does the following:
  - merge the two arrays in a result array
  - sort the result array
  - take the median from the sorted array

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] ar = new int[nums1.length + nums2.length];

        System.arraycopy(nums1, 0, ar, 0, nums1.length);
        System.arraycopy(nums2, 0, ar, nums1.length, nums2.length);

        Arrays.sort(ar);

        double median;
        if (ar.length % 2 == 0) {
            median = (double) (ar[ar.length / 2 - 1] + ar[ar.length / 2]) / 2;
        } else {
            median = ar[ar.length / 2];
        }
        
        return median;
    }
}
```

The time complexity for this appoach is O((m+n)log(m+n)) since the Arrays.sort() method has a worst case complexity of O(nlog(n)).

The space complexity is O(n+m) since we are storing all the values in a new array.
