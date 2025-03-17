# Product of Array Except Self

**Difficulty**: Medium  
**Tags**: Array, Prefix Sum

## Question
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

## Example Input/Output
**Input**: nums = [1,2,3,4]  
**Output**: [24,12,8,6]

**Input**: nums = [-1,1,0,-3,3]  
**Output**: [0,0,-9,0,0]

## Solution Algorithm
We can solve this problem by using two passes through the array:

1. First pass: Calculate the product of all elements to the left of each element.
2. Second pass: Calculate the product of all elements to the right of each element and multiply it with the left product.

### Approach:
1. Initialize an array `answer` of the same length as `nums`.
2. Set `answer[0] = 1` since there are no elements to the left of the first element.
3. Iterate from index 1 to n-1 and set `answer[i] = answer[i-1] * nums[i-1]` to calculate the product of all elements to the left.
4. Initialize a variable `rightProduct = 1` to keep track of the product of elements to the right.
5. Iterate from index n-1 to 0 and:
   - Multiply `answer[i]` with `rightProduct` to get the final result.
   - Update `rightProduct = rightProduct * nums[i]` for the next iteration.
6. Return the `answer` array.

## Solution Code
```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] answer = new int[n];
    
    // Calculate products of all elements to the left of each element
    answer[0] = 1; // No elements to the left of the first element
    for (int i = 1; i < n; i++) {
        answer[i] = answer[i - 1] * nums[i - 1];
    }
    
    // Calculate products of all elements to the right and multiply with left products
    int rightProduct = 1;
    for (int i = n - 1; i >= 0; i--) {
        answer[i] *= rightProduct;
        rightProduct *= nums[i];
    }
    
    return answer;
}
```

```java
// Alternative solution with O(1) extra space (excluding the output array)
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] answer = new int[n];
    
    // Initialize the answer array with 1s
    Arrays.fill(answer, 1);
    
    // Calculate products of all elements to the left of each element
    int leftProduct = 1;
    for (int i = 0; i < n; i++) {
        answer[i] *= leftProduct;
        leftProduct *= nums[i];
    }
    
    // Calculate products of all elements to the right and multiply with left products
    int rightProduct = 1;
    for (int i = n - 1; i >= 0; i--) {
        answer[i] *= rightProduct;
        rightProduct *= nums[i];
    }
    
    return answer;
}
``` 