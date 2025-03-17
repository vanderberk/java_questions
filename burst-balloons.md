# Burst Balloons

**Difficulty**: Hard  
**Tags**: Array, Dynamic Programming, Divide and Conquer

## Question
You are given `n` balloons, indexed from `0` to `n-1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i-1] * nums[i] * nums[i+1]` coins. If `i-1` or `i+1` goes out of bounds of the array, then treat it as if there is a balloon with a 1 painted on it.

Return the maximum coins you can collect by bursting the balloons wisely.

## Example Input/Output
**Input**: nums = [3,1,5,8]  
**Output**: 167  
**Explanation**:
- Burst balloon 1 (with value 1) to get 3 * 1 * 5 = 15 coins, nums becomes [3,5,8]
- Burst balloon 0 (with value 3) to get 1 * 3 * 5 = 15 coins, nums becomes [5,8]
- Burst balloon 0 (with value 5) to get 1 * 5 * 8 = 40 coins, nums becomes [8]
- Burst balloon 0 (with value 8) to get 1 * 8 * 1 = 8 coins, nums becomes []
- Total coins collected: 15 + 15 + 40 + 8 = 167

**Input**: nums = [1,5]  
**Output**: 10

## Solution Algorithm
This problem can be solved using dynamic programming. The key insight is to think about the problem in reverse: instead of considering which balloon to burst first, consider which balloon to burst last.

### Approach:
1. Add 1 at the beginning and end of the array to handle boundary cases.
2. Define `dp[i][j]` as the maximum coins that can be obtained by bursting all balloons between indices `i` and `j` (exclusive).
3. For each possible last balloon `k` between `i` and `j`, calculate the coins obtained:
   - Coins from bursting all balloons between `i` and `k`.
   - Coins from bursting all balloons between `k` and `j`.
   - Coins from bursting balloon `k` last: `nums[i] * nums[k] * nums[j]`.
4. The final answer is `dp[0][n+1]`, where `n` is the length of the original array.

## Solution Code
```java
public int maxCoins(int[] nums) {
    int n = nums.length;
    
    // Add 1 at the beginning and end of the array
    int[] newNums = new int[n + 2];
    newNums[0] = 1;
    newNums[n + 1] = 1;
    for (int i = 0; i < n; i++) {
        newNums[i + 1] = nums[i];
    }
    
    // Initialize dp array
    int[][] dp = new int[n + 2][n + 2];
    
    // Fill the dp array
    for (int len = 1; len <= n; len++) {
        for (int i = 1; i <= n - len + 1; i++) {
            int j = i + len - 1;
            for (int k = i; k <= j; k++) {
                // Calculate the maximum coins
                dp[i][j] = Math.max(dp[i][j], 
                                   dp[i][k-1] + dp[k+1][j] + newNums[i-1] * newNums[k] * newNums[j+1]);
            }
        }
    }
    
    return dp[1][n];
}
```

```java
// Alternative implementation with clearer variable names
public int maxCoins(int[] nums) {
    int n = nums.length;
    
    // Create a new array with 1s at the boundaries
    int[] balloons = new int[n + 2];
    balloons[0] = 1;
    balloons[n + 1] = 1;
    for (int i = 0; i < n; i++) {
        balloons[i + 1] = nums[i];
    }
    
    // dp[left][right] represents the maximum coins obtained by bursting all balloons
    // between indices left and right (exclusive)
    int[][] dp = new int[n + 2][n + 2];
    
    // Iterate over all possible subproblems
    for (int length = 1; length <= n; length++) {
        for (int left = 1; left <= n - length + 1; left++) {
            int right = left + length - 1;
            
            // Try each balloon as the last one to burst
            for (int last = left; last <= right; last++) {
                // Calculate the coins obtained
                int coinsFromLeft = dp[left][last - 1];  // Coins from bursting all balloons to the left of 'last'
                int coinsFromRight = dp[last + 1][right]; // Coins from bursting all balloons to the right of 'last'
                int coinsFromLast = balloons[left - 1] * balloons[last] * balloons[right + 1]; // Coins from bursting 'last'
                
                int totalCoins = coinsFromLeft + coinsFromLast + coinsFromRight;
                dp[left][right] = Math.max(dp[left][right], totalCoins);
            }
        }
    }
    
    return dp[1][n];
}
```

```java
// Top-down approach with memoization
public int maxCoins(int[] nums) {
    int n = nums.length;
    
    // Create a new array with 1s at the boundaries
    int[] balloons = new int[n + 2];
    balloons[0] = 1;
    balloons[n + 1] = 1;
    for (int i = 0; i < n; i++) {
        balloons[i + 1] = nums[i];
    }
    
    // Initialize memoization table
    Integer[][] memo = new Integer[n + 2][n + 2];
    
    return burst(balloons, memo, 1, n);
}

private int burst(int[] balloons, Integer[][] memo, int left, int right) {
    // Base case: no balloons to burst
    if (left > right) {
        return 0;
    }
    
    // Check if the result is already memoized
    if (memo[left][right] != null) {
        return memo[left][right];
    }
    
    int maxCoins = 0;
    
    // Try each balloon as the last one to burst
    for (int last = left; last <= right; last++) {
        // Calculate the coins obtained
        int coinsFromLeft = burst(balloons, memo, left, last - 1);
        int coinsFromRight = burst(balloons, memo, last + 1, right);
        int coinsFromLast = balloons[left - 1] * balloons[last] * balloons[right + 1];
        
        int totalCoins = coinsFromLeft + coinsFromLast + coinsFromRight;
        maxCoins = Math.max(maxCoins, totalCoins);
    }
    
    // Memoize the result
    memo[left][right] = maxCoins;
    return maxCoins;
}
``` 