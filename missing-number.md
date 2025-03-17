# Finding the Missing Number

**Difficulty**: Easy  
**Tags**: Arrays, Mathematics, Algorithms

## Question
Given an array containing n distinct numbers from 0 to n, find the missing number.

## Example Input/Output
**Input**: [3, 0, 1]  
**Output**: 2

**Input**: [9, 6, 4, 2, 3, 5, 7, 0, 1]  
**Output**: 8

## Solution Algorithm
1. Calculate the sum of numbers from 0 to n using the formula n*(n+1)/2
2. Calculate the sum of the numbers in the array
3. The difference between the expected sum and the actual sum is the missing number

## Solution Code
```java
public int missingNumber(int[] nums) {
    int n = nums.length;
    int expectedSum = n * (n + 1) / 2;
    
    int actualSum = 0;
    for (int num : nums) {
        actualSum += num;
    }
    
    return expectedSum - actualSum;
}
``` 