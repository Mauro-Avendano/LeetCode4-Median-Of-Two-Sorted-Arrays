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

Now it's evident that we can do it better, let's explore a new approach on which we will do the following:

  - Create an empty array of size (m+n)/2 + 1 which we will fill mantaining the ascending order.
  - Loop until the array is completelly filled
  - Compute the median depending if m+n is even or odd and return

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int totalL = nums1.length + nums2.length;
        int l = totalL/2 + 1;

        int[] res = new int[l];

        int i = 0;
        int j = 0;
        int k = 0;
        while (i < l) {
            if (j == nums1.length) {
                res[i] = nums2[k];
                k++;
            } else if (k == nums2.length) {
                res[i] = nums1[j];
                j++;
            } else if (nums1[j] < nums2[k]) {
                res[i] = nums1[j];
                j++;
            } else {
                res[i] = nums2[k];
                k++;
            }
            i++;
        }
        
        if (totalL % 2 == 0) return (double) (res[l - 1] + res[l - 2])/2;
        else return (double) res[l - 1];
    }
}
```

Here we are looping (m+n)/2 + 1 times so the time complexity is O(m+n) but surprisingly it performs really well with 1 ms of execution time on leetcode.
Space complexity is also O(m+n).

Now we will explore another solution using binary search. This approach is absurdly difficult to implement and very hard to figure it out how to use the binary search but basically we have to do the following:

 - Perform the binary search on the smaller array. ( O(log(n)) is better than O log(m) when n < m ).
 - **Do the binary search to determine how many elements of the smaller array are present on the merged array**.
 - Each array is partitioned based on the low and high values of the binary search.
 - Please note that the condition we are looking for is that the previous element of the partition of the nums1 array has to be less than or equal to the next element to the partition of the nums2 array and the previous element of the partition of the nums2 array has to be smaller or equal than the next element of the partition of the nums1 array. With this conditions we can be sure we have the rights partitions in order to compute the median. l1, l2, r1, r2 represents the left and right elements to the partitions of the arrays.
 - If the previous condition is not met, we discard the top half or bottom half of the search and continue iterating until we reach the desired value.

```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {        
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }
        
        int low = 0;
        int high = nums1.length;
        
        while (low <= high) {
            int partition1 = (low + high) / 2;
            int partition2 = (nums1.length + nums2.length + 1) / 2 - partition1;
            
            int l1 = (partition1 == 0) ? Integer.MIN_VALUE : nums1[partition1 - 1];
            int r1 = (partition1 == nums1.length) ? Integer.MAX_VALUE : nums1[partition1];
            int l2 = (partition2 == 0) ? Integer.MIN_VALUE : nums2[partition2 - 1];
            int r2 = (partition2 == nums2.length) ? Integer.MAX_VALUE : nums2[partition2];
            
            if (l1 <= r2 && l2 <= r1) {
                if ((nums1.length + nums2.length) % 2 == 0) {
                    return (double) (Math.max(l1, l2) + Math.min(r1, r2)) / 2;
                } else {
                    return (double) Math.max(l1, l2);
                }
            } else if (l1 > r2) {
                high = partition1 - 1;
            } else {
                low = partition1 + 1;
            }
        }
        return 0.0;
    }
}
```

