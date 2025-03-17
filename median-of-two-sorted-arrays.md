# Median of Two Sorted Arrays

**Difficulty**: Hard  
**Tags**: Arrays, Binary Search, Divide and Conquer

## Question
Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log(m+n)).

## Example Input/Output
**Input**: nums1 = [1,3], nums2 = [2]  
**Output**: 2.0  
**Explanation**: The merged array would be [1,2,3], and the median is 2.

**Input**: nums1 = [1,2], nums2 = [3,4]  
**Output**: 2.5  
**Explanation**: The merged array would be [1,2,3,4], and the median is (2 + 3) / 2 = 2.5.

## Solution Algorithm
1. Ensure nums1 is the smaller array for efficiency (if not, swap them)
2. Use binary search on the smaller array to find the correct partition point
3. Calculate the partition point for the second array based on the first array's partition
4. Check if the partition is valid:
   - maxLeft1 <= minRight2 and maxLeft2 <= minRight1
5. If the partition is valid, calculate the median based on the total length (odd or even)
6. If not valid, adjust the binary search range and try again

## Solution Code
```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    // Ensure nums1 is the smaller array
    if (nums1.length > nums2.length) {
        return findMedianSortedArrays(nums2, nums1);
    }
    
    int x = nums1.length;
    int y = nums2.length;
    
    int low = 0;
    int high = x;
    
    while (low <= high) {
        int partitionX = (low + high) / 2;
        int partitionY = (x + y + 1) / 2 - partitionX;
        
        // If partitionX is 0, use -INF for maxLeft1
        // If partitionX is length of input array, use +INF for minRight1
        int maxLeft1 = (partitionX == 0) ? Integer.MIN_VALUE : nums1[partitionX - 1];
        int minRight1 = (partitionX == x) ? Integer.MAX_VALUE : nums1[partitionX];
        
        int maxLeft2 = (partitionY == 0) ? Integer.MIN_VALUE : nums2[partitionY - 1];
        int minRight2 = (partitionY == y) ? Integer.MAX_VALUE : nums2[partitionY];
        
        if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1) {
            // We have found the correct partition
            
            // If total length is odd
            if ((x + y) % 2 != 0) {
                return Math.max(maxLeft1, maxLeft2);
            } else {
                // If total length is even
                return (Math.max(maxLeft1, maxLeft2) + Math.min(minRight1, minRight2)) / 2.0;
            }
        } else if (maxLeft1 > minRight2) {
            // Move partition to left
            high = partitionX - 1;
        } else {
            // Move partition to right
            low = partitionX + 1;
        }
    }
    
    // If we reach here, the arrays are not sorted
    throw new IllegalArgumentException("Input arrays are not sorted");
}
``` 