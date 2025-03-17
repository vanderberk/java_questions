# Maximum Subarray

**Difficulty**: Easy  
**Tags**: Arrays, Dynamic Programming, Divide and Conquer

## Question
Given an integer array, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

## Example Input/Output
**Input**: [-2, 1, -3, 4, -1, 2, 1, -5, 4]  
**Output**: 6  
**Explanation**: The contiguous subarray [4, -1, 2, 1] has the largest sum = 6.

**Input**: [1]  
**Output**: 1

## Solution Algorithm
1. Use Kadane's algorithm to find the maximum subarray sum
2. Initialize two variables: currentSum and maxSum, both set to the first element
3. Iterate through the array starting from the second element:
   - For each element, decide whether to start a new subarray or extend the existing one
   - currentSum = max(current element, currentSum + current element)
   - Update maxSum if currentSum is greater
4. Return maxSum

## Solution Code
```java
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    int currentSum = nums[0];
    int maxSum = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        currentSum = Math.max(nums[i], currentSum + nums[i]);
        maxSum = Math.max(maxSum, currentSum);
    }
    
    return maxSum;
}
``` 