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

Here we are looping (m+n)/2 + 1 times so the time complexity is O(m+n) but surprisingly it performs really well, check the image below.
Space complexity is also O(m+n).

[<img src="https://private-user-images.githubusercontent.com/5465728/339038636-7ef3f394-dcd7-408f-a12a-4d3feda7b0c5.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTgyMDcxMTUsIm5iZiI6MTcxODIwNjgxNSwicGF0aCI6Ii81NDY1NzI4LzMzOTAzODYzNi03ZWYzZjM5NC1kY2Q3LTQwOGYtYTEyYS00ZDNmZWRhN2IwYzUucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI0MDYxMiUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNDA2MTJUMTU0MDE1WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZGY0NmM2YmJmZTk5MTNiNDg0MTExZWM1ZjcxZDE0NzA1YzYwZTFlNWMyMmI2YTQ5NDMxNTcxMWZiMzRhNGMzNiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmYWN0b3JfaWQ9MCZrZXlfaWQ9MCZyZXBvX2lkPTAifQ.ZQrgI-8rCn6G6ZXSkY8YvANohrvUgcXBYlpQ-U2nFN4" width="500"/>](https://private-user-images.githubusercontent.com/5465728/339038636-7ef3f394-dcd7-408f-a12a-4d3feda7b0c5.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTgyMDcxMTUsIm5iZiI6MTcxODIwNjgxNSwicGF0aCI6Ii81NDY1NzI4LzMzOTAzODYzNi03ZWYzZjM5NC1kY2Q3LTQwOGYtYTEyYS00ZDNmZWRhN2IwYzUucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI0MDYxMiUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNDA2MTJUMTU0MDE1WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZGY0NmM2YmJmZTk5MTNiNDg0MTExZWM1ZjcxZDE0NzA1YzYwZTFlNWMyMmI2YTQ5NDMxNTcxMWZiMzRhNGMzNiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmYWN0b3JfaWQ9MCZrZXlfaWQ9MCZyZXBvX2lkPTAifQ.ZQrgI-8rCn6G6ZXSkY8YvANohrvUgcXBYlpQ-U2nFN4)


