# Remove Duplicates from Sorted Array

**Difficulty**: Easy  
**Tags**: Array, Two Pointers

## Question
Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` after placing the final result in the first `k` slots of `nums`.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.

## Example Input/Output
**Input**: nums = [1,1,2]  
**Output**: 2, nums = [1,2,_]  
**Explanation**: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively. It does not matter what you leave beyond the returned k (hence they are underscores).

**Input**: nums = [0,0,1,1,1,2,2,3,3,4]  
**Output**: 5, nums = [0,1,2,3,4,_,_,_,_,_]  
**Explanation**: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively. It does not matter what you leave beyond the returned k (hence they are underscores).

## Solution Algorithm
We can use the two-pointer technique to solve this problem:

1. If the array is empty, return 0
2. Initialize a pointer `i` to keep track of the position where the next unique element should be placed (starting at index 1)
3. Iterate through the array with another pointer `j` (starting from index 1):
   - If the current element at `j` is different from the element at `i-1`, place it at position `i` and increment `i`
   - Otherwise, skip the duplicate
4. Return `i` as the new length of the array with duplicates removed

## Solution Code
```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    
    // i is the position where the next unique element should be placed
    int i = 1;
    
    // Iterate through the array starting from the second element
    for (int j = 1; j < nums.length; j++) {
        // If current element is different from the previous one
        if (nums[j] != nums[j - 1]) {
            // Place it at position i and increment i
            nums[i] = nums[j];
            i++;
        }
        // If it's a duplicate, just skip it
    }
    
    // i is now the new length of the array without duplicates
    return i;
}
```

// Alternative implementation with clearer variable names
```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    
    int uniqueCount = 1; // We already have the first element
    
    for (int i = 1; i < nums.length; i++) {
        // If current element is different from the previous one
        if (nums[i] != nums[i - 1]) {
            // Place it at the next position in our unique elements section
            nums[uniqueCount] = nums[i];
            uniqueCount++;
        }
    }
    
    return uniqueCount;
}
``` 