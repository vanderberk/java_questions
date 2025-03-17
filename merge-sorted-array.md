# Merge Sorted Array

**Difficulty**: Easy  
**Tags**: Array, Two Pointers, Sorting

## Question
You are given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

Merge `nums1` and `nums2` into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to 0 and should be ignored. `nums2` has a length of `n`.

## Example Input/Output
**Input**: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3  
**Output**: [1,2,2,3,5,6]  
**Explanation**: The arrays we are merging are [1,2,3] and [2,5,6]. The result is [1,2,2,3,5,6].

**Input**: nums1 = [1], m = 1, nums2 = [], n = 0  
**Output**: [1]  
**Explanation**: The arrays we are merging are [1] and []. The result is [1].

**Input**: nums1 = [0], m = 0, nums2 = [1], n = 1  
**Output**: [1]  
**Explanation**: The arrays we are merging are [] and [1]. The result is [1].

## Solution Algorithm
The key insight is to fill the array from the end, which avoids overwriting elements in `nums1` that we still need to consider.

1. Initialize three pointers:
   - `p1` pointing to the last element in `nums1` (index `m-1`)
   - `p2` pointing to the last element in `nums2` (index `n-1`)
   - `p` pointing to the last position in the merged array (index `m+n-1`)
2. Compare elements at `p1` and `p2` and place the larger one at position `p`
3. Decrement the pointers accordingly
4. Continue until we've processed all elements from both arrays
5. If there are remaining elements in `nums2`, copy them to `nums1`

## Solution Code
```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    // Initialize pointers to the last elements of both arrays
    int p1 = m - 1;     // Last element in nums1
    int p2 = n - 1;     // Last element in nums2
    int p = m + n - 1;  // Last position in the merged array
    
    // While there are elements in both arrays
    while (p1 >= 0 && p2 >= 0) {
        // Compare and place the larger element at the end of nums1
        if (nums1[p1] > nums2[p2]) {
            nums1[p] = nums1[p1];
            p1--;
        } else {
            nums1[p] = nums2[p2];
            p2--;
        }
        p--;
    }
    
    // If there are remaining elements in nums2, copy them to nums1
    // (No need to handle remaining elements in nums1 as they are already in place)
    while (p2 >= 0) {
        nums1[p] = nums2[p2];
        p2--;
        p--;
    }
}
```

// Alternative implementation with cleaner code
```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int p1 = m - 1;
    int p2 = n - 1;
    int p = m + n - 1;
    
    // Start from the end and work backwards
    while (p >= 0) {
        // If p2 < 0, we've used all elements from nums2
        // If p1 >= 0 and nums1[p1] > nums2[p2], use element from nums1
        if (p2 < 0 || (p1 >= 0 && nums1[p1] > nums2[p2])) {
            nums1[p--] = nums1[p1--];
        } else {
            nums1[p--] = nums2[p2--];
        }
    }
}
``` 