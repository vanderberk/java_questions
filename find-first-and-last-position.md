# Find First and Last Position of Element in Sorted Array

**Difficulty**: Medium  
**Tags**: Array, Binary Search

## Question
Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

## Example Input/Output
**Input**: nums = [5,7,7,8,8,10], target = 8  
**Output**: [3,4]

**Input**: nums = [5,7,7,8,8,10], target = 6  
**Output**: [-1,-1]

**Input**: nums = [], target = 0  
**Output**: [-1,-1]

## Solution Algorithm
We can solve this problem using binary search:

1. Implement a binary search function to find the leftmost (first) occurrence of the target.
2. Implement a binary search function to find the rightmost (last) occurrence of the target.
3. Combine the results to get the range.

### Finding the Leftmost Position
1. Perform a binary search, but when we find the target, we continue searching in the left half to find the leftmost occurrence.
2. If the target is not found, return -1.

### Finding the Rightmost Position
1. Perform a binary search, but when we find the target, we continue searching in the right half to find the rightmost occurrence.
2. If the target is not found, return -1.

## Solution Code
```java
public int[] searchRange(int[] nums, int target) {
    int[] result = {-1, -1};
    
    if (nums == null || nums.length == 0) {
        return result;
    }
    
    // Find the leftmost position
    result[0] = findLeftmost(nums, target);
    
    // If target is not found, return [-1, -1]
    if (result[0] == -1) {
        return result;
    }
    
    // Find the rightmost position
    result[1] = findRightmost(nums, target);
    
    return result;
}

private int findLeftmost(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            result = mid;  // Update result
            right = mid - 1;  // Continue searching in the left half
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

private int findRightmost(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            result = mid;  // Update result
            left = mid + 1;  // Continue searching in the right half
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

// Alternative implementation with a single binary search function
```java
public int[] searchRange(int[] nums, int target) {
    int[] result = {-1, -1};
    
    if (nums == null || nums.length == 0) {
        return result;
    }
    
    // Find the leftmost position
    result[0] = binarySearch(nums, target, true);
    
    // If target is not found, return [-1, -1]
    if (result[0] == -1) {
        return result;
    }
    
    // Find the rightmost position
    result[1] = binarySearch(nums, target, false);
    
    return result;
}

private int binarySearch(int[] nums, int target, boolean findLeftmost) {
    int left = 0;
    int right = nums.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            result = mid;  // Update result
            
            if (findLeftmost) {
                right = mid - 1;  // Continue searching in the left half
            } else {
                left = mid + 1;  // Continue searching in the right half
            }
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
``` 