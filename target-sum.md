# Target Sum

**Difficulty**: Medium  
**Tags**: Array, Dynamic Programming, Backtracking, Depth-First Search

## Question
You are given an integer array `nums` and an integer `target`.

You want to build an expression out of `nums` by adding one of the symbols `+` and `-` before each integer in `nums` and then concatenate all the integers.

For example, if `nums = [2, 1]`, you can add a `+` before 2 and a `-` before 1 and concatenate them to build the expression `+2-1`.

Return the number of different expressions that you can build, which evaluates to `target`.

## Example Input/Output
**Input**: nums = [1,1,1,1,1], target = 3  
**Output**: 5  
**Explanation**: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

**Input**: nums = [1], target = 1  
**Output**: 1

## Solution Algorithm
This problem can be solved using several approaches:

### Approach 1: Depth-First Search (DFS)
1. Use recursion to explore all possible combinations of `+` and `-` signs.
2. For each number in the array, we have two choices: add it or subtract it.
3. When we've processed all numbers, check if the sum equals the target.

### Approach 2: Dynamic Programming
1. Transform the problem into a subset sum problem:
   - Let P be the sum of elements assigned with `+` sign.
   - Let N be the sum of elements assigned with `-` sign.
   - Then P + N = sum(nums) and P - N = target.
   - Solving these equations: P = (sum(nums) + target) / 2.
2. The problem becomes: find the number of subsets with sum equal to P.
3. Use dynamic programming to solve this subset sum problem.

## Solution Code
```java
// Approach 1: DFS (Recursive)
public int findTargetSumWays(int[] nums, int target) {
    return dfs(nums, target, 0, 0);
}

private int dfs(int[] nums, int target, int index, int currentSum) {
    // Base case: if we've processed all numbers
    if (index == nums.length) {
        // Return 1 if we've reached the target, 0 otherwise
        return currentSum == target ? 1 : 0;
    }
    
    // Explore adding the current number
    int addWays = dfs(nums, target, index + 1, currentSum + nums[index]);
    
    // Explore subtracting the current number
    int subtractWays = dfs(nums, target, index + 1, currentSum - nums[index]);
    
    // Return the total number of ways
    return addWays + subtractWays;
}
```

```java
// Approach 1: DFS with Memoization
public int findTargetSumWays(int[] nums, int target) {
    // Use a map to store computed results
    Map<String, Integer> memo = new HashMap<>();
    return dfs(nums, target, 0, 0, memo);
}

private int dfs(int[] nums, int target, int index, int currentSum, Map<String, Integer> memo) {
    // Base case: if we've processed all numbers
    if (index == nums.length) {
        return currentSum == target ? 1 : 0;
    }
    
    // Create a key for memoization
    String key = index + "," + currentSum;
    
    // Check if we've already computed this result
    if (memo.containsKey(key)) {
        return memo.get(key);
    }
    
    // Explore adding the current number
    int addWays = dfs(nums, target, index + 1, currentSum + nums[index], memo);
    
    // Explore subtracting the current number
    int subtractWays = dfs(nums, target, index + 1, currentSum - nums[index], memo);
    
    // Store the result in the memo
    int totalWays = addWays + subtractWays;
    memo.put(key, totalWays);
    
    return totalWays;
}
```

```java
// Approach 2: Dynamic Programming (Subset Sum)
public int findTargetSumWays(int[] nums, int target) {
    int sum = 0;
    for (int num : nums) {
        sum += num;
    }
    
    // If the target is out of range or (sum + target) is odd, return 0
    if (Math.abs(target) > sum || (sum + target) % 2 != 0) {
        return 0;
    }
    
    // Calculate the subset sum we need to find
    int subsetSum = (sum + target) / 2;
    
    // Use DP to find the number of subsets with the given sum
    int[] dp = new int[subsetSum + 1];
    dp[0] = 1; // Empty subset has sum 0
    
    for (int num : nums) {
        for (int j = subsetSum; j >= num; j--) {
            dp[j] += dp[j - num];
        }
    }
    
    return dp[subsetSum];
}
``` 