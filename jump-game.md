# Jump Game

**Difficulty**: Medium  
**Tags**: Array, Dynamic Programming, Greedy

## Question
You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

## Example Input/Output
**Input**: nums = [2,3,1,1,4]  
**Output**: true  
**Explanation**: Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Input**: nums = [3,2,1,0,4]  
**Output**: false  
**Explanation**: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

## Solution Algorithm
This problem can be solved using several approaches:

### Approach 1: Greedy
1. Start from the last index and work backwards.
2. For each position, check if it can reach a position that can reach the end.
3. If a position can reach the end, mark it as a "good" position.
4. Continue until we reach the first position.
5. If the first position is a "good" position, return true.

### Approach 2: Dynamic Programming
1. Create a DP array where `dp[i]` indicates whether we can reach the end from position `i`.
2. Initialize `dp[n-1] = true` (we can reach the end from the end).
3. For each position from `n-2` to `0`:
   - Check if we can reach any position `j` such that `dp[j]` is true.
   - If yes, set `dp[i] = true`.
4. Return `dp[0]`.

### Approach 3: Greedy (Forward)
1. Keep track of the furthest position we can reach.
2. Iterate through the array and update the furthest position.
3. If at any point, the current position is beyond the furthest we can reach, return false.
4. If the furthest position is greater than or equal to the last index, return true.

## Solution Code
```java
// Approach 1: Greedy (Backward)
public boolean canJump(int[] nums) {
    int n = nums.length;
    int lastPos = n - 1;
    
    for (int i = n - 2; i >= 0; i--) {
        if (i + nums[i] >= lastPos) {
            lastPos = i;
        }
    }
    
    return lastPos == 0;
}
```

```java
// Approach 2: Dynamic Programming
public boolean canJump(int[] nums) {
    int n = nums.length;
    boolean[] dp = new boolean[n];
    dp[n - 1] = true;
    
    for (int i = n - 2; i >= 0; i--) {
        int furthestJump = Math.min(i + nums[i], n - 1);
        for (int j = i + 1; j <= furthestJump; j++) {
            if (dp[j]) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[0];
}
```

```java
// Approach 3: Greedy (Forward)
public boolean canJump(int[] nums) {
    int n = nums.length;
    int furthest = 0;
    
    for (int i = 0; i < n; i++) {
        // If current position is beyond the furthest we can reach
        if (i > furthest) {
            return false;
        }
        
        // Update the furthest position we can reach
        furthest = Math.max(furthest, i + nums[i]);
        
        // If we can reach the last index, return true
        if (furthest >= n - 1) {
            return true;
        }
    }
    
    return true; // This line is actually unreachable
}
```

```java
// Approach 4: BFS (less efficient but illustrative)
public boolean canJump(int[] nums) {
    int n = nums.length;
    boolean[] visited = new boolean[n];
    Queue<Integer> queue = new LinkedList<>();
    
    queue.offer(0);
    visited[0] = true;
    
    while (!queue.isEmpty()) {
        int current = queue.poll();
        
        if (current == n - 1) {
            return true;
        }
        
        for (int jump = 1; jump <= nums[current]; jump++) {
            int next = current + jump;
            
            if (next < n && !visited[next]) {
                queue.offer(next);
                visited[next] = true;
            }
        }
    }
    
    return false;
}
``` 