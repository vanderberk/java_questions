# Two Sum

**Difficulty**: Easy  
**Tags**: Arrays, Hash Table

## Question
Given an array of integers and a target sum, return the indices of two numbers such that they add up to the target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

## Example Input/Output
**Input**: nums = [2, 7, 11, 15], target = 9  
**Output**: [0, 1]  
**Explanation**: Because nums[0] + nums[1] = 2 + 7 = 9

**Input**: nums = [3, 2, 4], target = 6  
**Output**: [1, 2]

## Solution Algorithm
1. Create a hash map to store numbers and their indices
2. Iterate through the array:
   - For each number, calculate its complement (target - current number)
   - If the complement exists in the hash map, return the current index and the complement's index
   - Otherwise, add the current number and its index to the hash map
3. If no solution is found, return an empty array

## Solution Code
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> numMap = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        
        if (numMap.containsKey(complement)) {
            return new int[] {numMap.get(complement), i};
        }
        
        numMap.put(nums[i], i);
    }
    
    return new int[] {}; // No solution found
}
``` 