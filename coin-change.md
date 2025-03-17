# Coin Change

**Difficulty**: Medium  
**Tags**: Array, Dynamic Programming, Breadth-First Search

## Question
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

## Example Input/Output
**Input**: coins = [1,2,5], amount = 11  
**Output**: 3  
**Explanation**: 11 = 5 + 5 + 1

**Input**: coins = [2], amount = 3  
**Output**: -1  
**Explanation**: It is impossible to make 3 with given coins.

**Input**: coins = [1], amount = 0  
**Output**: 0  
**Explanation**: No coins needed to make 0.

## Solution Algorithm
This is a classic dynamic programming problem. We can solve it using the following approaches:

### Approach 1: Dynamic Programming (Bottom-Up)
1. Create a DP array `dp` of size `amount + 1` where `dp[i]` represents the minimum number of coins needed to make up amount `i`.
2. Initialize `dp[0] = 0` (it takes 0 coins to make up amount 0).
3. Initialize all other `dp[i]` to a value greater than the maximum possible value (e.g., `amount + 1`).
4. For each coin denomination and for each amount from the coin value to the target amount:
   - Update `dp[amount]` by taking the minimum of the current value and `dp[amount - coin] + 1`.
5. If `dp[amount]` is still greater than the maximum possible value, return `-1`. Otherwise, return `dp[amount]`.

### Approach 2: Breadth-First Search (BFS)
1. Use BFS to find the shortest path from 0 to the target amount.
2. Start with amount 0 in the queue and a visited set.
3. For each amount in the queue, try adding each coin denomination.
4. If the new amount equals the target, return the current level.
5. If the new amount is less than the target and not visited, add it to the queue and mark it as visited.
6. If the queue becomes empty without finding the target, return `-1`.

## Solution Code
```java
// Approach 1: Dynamic Programming (Bottom-Up)
public int coinChange(int[] coins, int amount) {
    // Initialize dp array
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1); // Fill with a value greater than the maximum possible
    dp[0] = 0; // Base case: 0 coins needed to make amount 0
    
    // Build the dp array
    for (int coin : coins) {
        for (int i = coin; i <= amount; i++) {
            dp[i] = Math.min(dp[i], dp[i - coin] + 1);
        }
    }
    
    // Check if a solution exists
    return dp[amount] > amount ? -1 : dp[amount];
}
```

```java
// Approach 2: Breadth-First Search (BFS)
public int coinChange(int[] coins, int amount) {
    if (amount == 0) {
        return 0;
    }
    
    Queue<Integer> queue = new LinkedList<>();
    Set<Integer> visited = new HashSet<>();
    
    queue.offer(0);
    visited.add(0);
    
    int level = 0;
    
    while (!queue.isEmpty()) {
        int size = queue.size();
        level++;
        
        for (int i = 0; i < size; i++) {
            int currentAmount = queue.poll();
            
            for (int coin : coins) {
                int nextAmount = currentAmount + coin;
                
                if (nextAmount == amount) {
                    return level;
                }
                
                if (nextAmount < amount && !visited.contains(nextAmount)) {
                    queue.offer(nextAmount);
                    visited.add(nextAmount);
                }
            }
        }
    }
    
    return -1;
}
```

```java
// Alternative DP approach with memoization (Top-Down)
public int coinChange(int[] coins, int amount) {
    if (amount < 1) {
        return 0;
    }
    
    return coinChangeHelper(coins, amount, new int[amount + 1]);
}

private int coinChangeHelper(int[] coins, int remaining, int[] memo) {
    if (remaining < 0) {
        return -1;
    }
    
    if (remaining == 0) {
        return 0;
    }
    
    if (memo[remaining] != 0) {
        return memo[remaining];
    }
    
    int min = Integer.MAX_VALUE;
    for (int coin : coins) {
        int result = coinChangeHelper(coins, remaining - coin, memo);
        if (result >= 0 && result < min) {
            min = result + 1;
        }
    }
    
    memo[remaining] = (min == Integer.MAX_VALUE) ? -1 : min;
    return memo[remaining];
}