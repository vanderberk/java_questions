# Sort Colors

**Difficulty**: Medium  
**Tags**: Array, Two Pointers, Sorting

## Question
Given an array `nums` with `n` objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

## Example Input/Output
**Input**: nums = [2,0,2,1,1,0]  
**Output**: [0,0,1,1,2,2]

**Input**: nums = [2,0,1]  
**Output**: [0,1,2]

## Solution Algorithm
This problem is also known as the Dutch National Flag problem, and we can solve it using the three-pointer approach:

1. Initialize three pointers:
   - `low` pointing to the beginning of the array (where 0s should be placed)
   - `mid` pointing to the current element being examined
   - `high` pointing to the end of the array (where 2s should be placed)
2. Iterate through the array with the `mid` pointer:
   - If `nums[mid]` is 0, swap it with `nums[low]` and increment both `low` and `mid`
   - If `nums[mid]` is 1, just increment `mid`
   - If `nums[mid]` is 2, swap it with `nums[high]` and decrement `high` (don't increment `mid` yet, as the swapped element needs to be examined)
3. Continue until `mid` crosses `high`

## Solution Code
```java
public void sortColors(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    
    int low = 0;        // Pointer for 0s (red)
    int mid = 0;        // Pointer for 1s (white)
    int high = nums.length - 1;  // Pointer for 2s (blue)
    
    while (mid <= high) {
        if (nums[mid] == 0) {
            // Swap with the low pointer and move both pointers
            swap(nums, low, mid);
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            // Just move the mid pointer
            mid++;
        } else if (nums[mid] == 2) {
            // Swap with the high pointer and move only the high pointer
            swap(nums, mid, high);
            high--;
            // Don't increment mid here because the swapped element needs to be examined
        }
    }
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```

// Alternative implementation using counting sort
```java
public void sortColors(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    
    // Count the occurrences of each color
    int count0 = 0;
    int count1 = 0;
    int count2 = 0;
    
    for (int num : nums) {
        if (num == 0) {
            count0++;
        } else if (num == 1) {
            count1++;
        } else if (num == 2) {
            count2++;
        }
    }
    
    // Fill the array with the correct colors
    int index = 0;
    
    // Fill with 0s
    for (int i = 0; i < count0; i++) {
        nums[index++] = 0;
    }
    
    // Fill with 1s
    for (int i = 0; i < count1; i++) {
        nums[index++] = 1;
    }
    
    // Fill with 2s
    for (int i = 0; i < count2; i++) {
        nums[index++] = 2;
    }
}
``` 