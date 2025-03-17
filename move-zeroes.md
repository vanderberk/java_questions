# Move Zeroes

**Difficulty**: Easy  
**Tags**: Array, Two Pointers

## Question
Given an integer array `nums`, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

**Note**: You must do this in-place without making a copy of the array.

## Example Input/Output
**Input**: nums = [0,1,0,3,12]  
**Output**: [1,3,12,0,0]

**Input**: nums = [0]  
**Output**: [0]

## Solution Algorithm
We can solve this problem using the two-pointer technique:

1. Initialize a pointer `insertPos` to keep track of where the next non-zero element should be placed
2. Iterate through the array:
   - When we find a non-zero element, swap it with the element at `insertPos` and increment `insertPos`
   - When we find a zero, just skip it
3. After the iteration, all non-zero elements are in their correct positions, and all positions after `insertPos` will be zeros

## Solution Code
```java
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    
    // Position where the next non-zero element should be placed
    int insertPos = 0;
    
    // Iterate through the array
    for (int i = 0; i < nums.length; i++) {
        // If the current element is non-zero
        if (nums[i] != 0) {
            // Swap it with the element at insertPos
            int temp = nums[insertPos];
            nums[insertPos] = nums[i];
            nums[i] = temp;
            
            // Increment insertPos
            insertPos++;
        }
    }
    
    // At this point, all non-zero elements are at the beginning of the array
    // and all zeros are at the end
}
```

// Alternative implementation without swapping (more efficient)
```java
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    
    // Position where the next non-zero element should be placed
    int insertPos = 0;
    
    // First pass: place all non-zero elements at the beginning
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            nums[insertPos++] = nums[i];
        }
    }
    
    // Second pass: fill the remaining positions with zeros
    while (insertPos < nums.length) {
        nums[insertPos++] = 0;
    }
}
```

// One more alternative with minimal operations
```java
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    
    // Position where the next non-zero element should be placed
    int insertPos = 0;
    
    // Iterate through the array
    for (int i = 0; i < nums.length; i++) {
        // If the current element is non-zero
        if (nums[i] != 0) {
            // Only swap if i and insertPos are different positions
            if (i != insertPos) {
                nums[insertPos] = nums[i];
                nums[i] = 0;
            }
            insertPos++;
        }
    }
}
``` 