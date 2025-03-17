# House Robber

**Difficulty**: Medium  
**Tags**: Array, Dynamic Programming

## Question
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

## Example Input/Output
**Input**: nums = [1,2,3,1]  
**Output**: 4  
**Explanation**: Rob house 1 (money = 1) and then rob house 3 (money = 3). Total amount you can rob = 1 + 3 = 4.

**Input**: nums = [2,7,9,3,1]  
**Output**: 12  
**Explanation**: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1). Total amount you can rob = 2 + 9 + 1 = 12.

## Solution Algorithm
This is a classic dynamic programming problem. We need to decide whether to rob the current house or skip it.

1. Define the state: Let `dp[i]` be the maximum amount of money that can be robbed up to house `i`.
2. Define the base cases:
   - `dp[0] = nums[0]` (if there's only one house, rob it)
   - `dp[1] = max(nums[0], nums[1])` (if there are two houses, rob the one with more money)
3. Define the state transition:
   - For each house `i` (starting from index 2), we have two options:
     - Rob the current house: `nums[i] + dp[i-2]` (we can't rob the previous house)
     - Skip the current house: `dp[i-1]` (take the maximum amount from the previous house)
   - `dp[i] = max(nums[i] + dp[i-2], dp[i-1])`
4. Return `dp[n-1]`, where `n` is the length of the array.

We can optimize the space complexity to O(1) by only keeping track of the two previous states.

## Solution Code
```java
public int rob(int[] nums) {
    int n = nums.length;
    
    if (n == 0) {
        return 0;
    }
    
    if (n == 1) {
        return nums[0];
    }
    
    // Initialize dp array
    int[] dp = new int[n];
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);
    
    // Fill the dp array
    for (int i = 2; i < n; i++) {
        dp[i] = Math.max(nums[i] + dp[i - 2], dp[i - 1]);
    }
    
    return dp[n - 1];
}
```

```java
// Space-optimized solution
public int rob(int[] nums) {
    int n = nums.length;
    
    if (n == 0) {
        return 0;
    }
    
    if (n == 1) {
        return nums[0];
    }
    
    // Initialize variables to store the two previous states
    int prevTwo = nums[0];
    int prevOne = Math.max(nums[0], nums[1]);
    
    // Iterate through the array starting from the third house
    for (int i = 2; i < n; i++) {
        int current = Math.max(nums[i] + prevTwo, prevOne);
        prevTwo = prevOne;
        prevOne = current;
    }
    
    return prevOne;
}
```

```java
// Alternative implementation with more descriptive variable names
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    int n = nums.length;
    
    if (n == 1) {
        return nums[0];
    }
    
    // maxRobbedAmount[i] represents the maximum amount that can be robbed
    // from the first i houses
    int[] maxRobbedAmount = new int[n];
    
    // Base cases
    maxRobbedAmount[0] = nums[0];
    maxRobbedAmount[1] = Math.max(nums[0], nums[1]);
    
    // Build up the solution
    for (int i = 2; i < n; i++) {
        // Either rob the current house and add to the amount robbed from houses before the previous one,
        // or skip the current house and take the maximum amount from the previous house
        maxRobbedAmount[i] = Math.max(nums[i] + maxRobbedAmount[i - 2], maxRobbedAmount[i - 1]);
    }
    
    return maxRobbedAmount[n - 1];
}
``` 