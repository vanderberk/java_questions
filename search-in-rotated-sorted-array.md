# Search in Rotated Sorted Array

**Difficulty**: Medium  
**Tags**: Array, Binary Search, Divide and Conquer

## Question
There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k` (1 <= k < nums.length) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index 3 and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with O(log n) runtime complexity.

## Example Input/Output
**Input**: nums = [4,5,6,7,0,1,2], target = 0  
**Output**: 4

**Input**: nums = [4,5,6,7,0,1,2], target = 3  
**Output**: -1

**Input**: nums = [1], target = 0  
**Output**: -1

## Solution Algorithm
1. Use a modified binary search algorithm to handle the rotation
2. Find the middle element of the array
3. Determine which half of the array is sorted (at least one half will always be sorted)
4. Check if the target is in the sorted half:
   - If yes, search in that half
   - If no, search in the other half
5. Continue the binary search until the target is found or the search space is empty

## Solution Code
```java
public int search(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return -1;
    }
    
    int left = 0;
    int right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        // Found the target
        if (nums[mid] == target) {
            return mid;
        }
        
        // Check if the left half is sorted
        if (nums[left] <= nums[mid]) {
            // Check if target is in the left half
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1; // Search in the left half
            } else {
                left = mid + 1; // Search in the right half
            }
        } 
        // Right half is sorted
        else {
            // Check if target is in the right half
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1; // Search in the right half
            } else {
                right = mid - 1; // Search in the left half
            }
        }
    }
    
    return -1; // Target not found
}
```

// Alternative implementation with clearer logic
```java
public int search(int[] nums, int target) {
    int start = 0;
    int end = nums.length - 1;
    
    while (start <= end) {
        int mid = start + (end - start) / 2;
        
        if (nums[mid] == target) {
            return mid;
        }
        
        // Check which half is sorted
        boolean leftHalfSorted = nums[start] <= nums[mid];
        
        if (leftHalfSorted) {
            // Left half is sorted
            boolean targetInLeftHalf = nums[start] <= target && target < nums[mid];
            if (targetInLeftHalf) {
                end = mid - 1;
            } else {
                start = mid + 1;
            }
        } else {
            // Right half is sorted
            boolean targetInRightHalf = nums[mid] < target && target <= nums[end];
            if (targetInRightHalf) {
                start = mid + 1;
            } else {
                end = mid - 1;
            }
        }
    }
    
    return -1;
}
``` 