# Climbing Stairs

**Difficulty**: Easy  
**Tags**: Dynamic Programming, Math, Memoization

## Question
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

## Example Input/Output
**Input**: n = 2  
**Output**: 2  
**Explanation**: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

**Input**: n = 3  
**Output**: 3  
**Explanation**: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

## Solution Algorithm
This problem can be solved using dynamic programming. The key insight is that to reach the nth step, you can either:
1. Take a single step from the (n-1)th step, or
2. Take a double step from the (n-2)th step

Therefore, the total number of ways to reach the nth step is the sum of the ways to reach the (n-1)th step and the (n-2)th step.

This forms a Fibonacci sequence where:
- dp[1] = 1 (one way to reach the first step)
- dp[2] = 2 (two ways to reach the second step)
- dp[n] = dp[n-1] + dp[n-2] for n > 2

We can implement this using:
1. **Recursive approach with memoization**: To avoid recalculating the same subproblems
2. **Iterative approach with tabulation**: Building up the solution from the bottom
3. **Optimized iterative approach**: Using only two variables instead of an array

## Solution Code
```java
// Approach 1: Recursive with Memoization
public int climbStairs(int n) {
    int[] memo = new int[n + 1];
    return climbStairsRecursive(n, memo);
}

private int climbStairsRecursive(int n, int[] memo) {
    if (n <= 2) {
        return n;
    }
    
    if (memo[n] != 0) {
        return memo[n];
    }
    
    memo[n] = climbStairsRecursive(n - 1, memo) + climbStairsRecursive(n - 2, memo);
    return memo[n];
}
```

```java
// Approach 2: Dynamic Programming with Tabulation
public int climbStairs(int n) {
    if (n <= 2) {
        return n;
    }
    
    int[] dp = new int[n + 1];
    dp[1] = 1;
    dp[2] = 2;
    
    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

```java
// Approach 3: Optimized Space Complexity O(1)
public int climbStairs(int n) {
    if (n <= 2) {
        return n;
    }
    
    int first = 1;  // Ways to climb 1 step
    int second = 2; // Ways to climb 2 steps
    int result = 0;
    
    for (int i = 3; i <= n; i++) {
        result = first + second;
        first = second;
        second = result;
    }
    
    return second;
}
``` 