# Next Permutation

**Difficulty**: Medium  
**Tags**: Array, Two Pointers, Algorithm

## Question
Implement the next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be in place and use only constant extra memory.

## Example Input/Output
**Input**: nums = [1,2,3]  
**Output**: [1,3,2]

**Input**: nums = [3,2,1]  
**Output**: [1,2,3]

**Input**: nums = [1,1,5]  
**Output**: [1,5,1]

## Solution Algorithm
The algorithm to find the next permutation can be described in the following steps:

1. Find the largest index `i` such that `nums[i] < nums[i + 1]`. If no such index exists, the permutation is the last permutation.
2. Find the largest index `j > i` such that `nums[j] > nums[i]`.
3. Swap `nums[i]` and `nums[j]`.
4. Reverse the sub-array starting at `nums[i + 1]`.

This algorithm ensures that we find the next lexicographically greater permutation in O(n) time complexity and with O(1) extra space.

## Solution Code
```java
public void nextPermutation(int[] nums) {
    // Find the first decreasing element from the right
    int i = nums.length - 2;
    while (i >= 0 && nums[i] >= nums[i + 1]) {
        i--;
    }
    
    // If such element exists
    if (i >= 0) {
        // Find the element just larger than nums[i]
        int j = nums.length - 1;
        while (nums[j] <= nums[i]) {
            j--;
        }
        
        // Swap nums[i] and nums[j]
        swap(nums, i, j);
    }
    
    // Reverse the subarray starting at nums[i+1]
    reverse(nums, i + 1, nums.length - 1);
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}

private void reverse(int[] nums, int start, int end) {
    while (start < end) {
        swap(nums, start, end);
        start++;
        end--;
    }
}
```

```java
// Alternative implementation with more detailed comments
public void nextPermutation(int[] nums) {
    int n = nums.length;
    
    // Step 1: Find the first decreasing element from the right
    int i = n - 2;
    while (i >= 0 && nums[i] >= nums[i + 1]) {
        i--;
    }
    
    // If we found a decreasing element
    if (i >= 0) {
        // Step 2: Find the element just larger than nums[i]
        int j = n - 1;
        while (j >= 0 && nums[j] <= nums[i]) {
            j--;
        }
        
        // Step 3: Swap nums[i] and nums[j]
        swap(nums, i, j);
    }
    
    // Step 4: Reverse the subarray starting at nums[i+1]
    // If i = -1, this will reverse the entire array (for the case when the array is in descending order)
    reverse(nums, i + 1, n - 1);
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}

private void reverse(int[] nums, int start, int end) {
    while (start < end) {
        swap(nums, start, end);
        start++;
        end--;
    }
}
```

```java
// Implementation with visualization comments
public void nextPermutation(int[] nums) {
    // Example: [1,2,3] -> [1,3,2]
    // Example: [3,2,1] -> [1,2,3]
    // Example: [1,1,5] -> [1,5,1]
    
    int n = nums.length;
    
    // Step 1: Find the first pair of adjacent elements (i, i+1) from right to left where nums[i] < nums[i+1]
    // For [1,2,3], we find i=1 (value 2) because 2 < 3
    // For [3,2,1], we don't find such i, so i becomes -1
    // For [1,1,5], we find i=1 (value 1) because 1 < 5
    int i = n - 2;
    while (i >= 0 && nums[i] >= nums[i + 1]) {
        i--;
    }
    
    // If we found such a pair
    if (i >= 0) {
        // Step 2: Find the smallest element to the right of i that is greater than nums[i]
        // For [1,2,3], j=2 (value 3) because 3 > 2
        // For [1,1,5], j=2 (value 5) because 5 > 1
        int j = n - 1;
        while (nums[j] <= nums[i]) {
            j--;
        }
        
        // Step 3: Swap nums[i] and nums[j]
        // [1,2,3] becomes [1,3,2]
        // [1,1,5] becomes [1,5,1]
        swap(nums, i, j);
    }
    
    // Step 4: Reverse the subarray starting at i+1
    // For [1,3,2], we reverse from index 2, which doesn't change anything
    // For [3,2,1], we reverse the entire array to get [1,2,3]
    // For [1,5,1], we reverse from index 2, which doesn't change anything
    reverse(nums, i + 1, n - 1);
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}

private void reverse(int[] nums, int start, int end) {
    while (start < end) {
        swap(nums, start, end);
        start++;
        end--;
    }
}
``` 