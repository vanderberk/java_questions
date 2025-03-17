# Longest Increasing Subsequence

**Difficulty**: Medium  
**Tags**: Array, Binary Search, Dynamic Programming

## Question
Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

## Example Input/Output
**Input**: nums = [10,9,2,5,3,7,101,18]  
**Output**: 4  
**Explanation**: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Input**: nums = [0,1,0,3,2,3]  
**Output**: 4  
**Explanation**: The longest increasing subsequence is [0,1,2,3], therefore the length is 4.

**Input**: nums = [7,7,7,7,7,7,7]  
**Output**: 1  
**Explanation**: The longest increasing subsequence is [7], therefore the length is 1.

## Solution Algorithm
There are two main approaches to solve this problem:

### Approach 1: Dynamic Programming (O(n²))
1. Create a DP array where `dp[i]` represents the length of the longest increasing subsequence ending at index `i`.
2. Initialize all elements in the DP array to 1 (as each element by itself is a subsequence of length 1).
3. For each element at index `i`, look back at all previous elements at indices `j < i`:
   - If `nums[i] > nums[j]`, then we can extend the subsequence ending at `j` to include the element at `i`.
   - Update `dp[i] = max(dp[i], dp[j] + 1)`.
4. The answer is the maximum value in the DP array.

### Approach 2: Dynamic Programming with Binary Search (O(n log n))
1. Maintain a list `tails` where `tails[i]` represents the smallest ending value of all increasing subsequences of length `i+1`.
2. For each element in the array:
   - Use binary search to find the position where the current element would fit in the `tails` array.
   - If the element is larger than all ending values, append it to the `tails` array.
   - Otherwise, replace the smallest ending value that is greater than or equal to the current element.
3. The length of the `tails` array is the length of the longest increasing subsequence.

## Solution Code
```java
// Approach 1: Dynamic Programming (O(n²))
public int lengthOfLIS(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    int n = nums.length;
    int[] dp = new int[n];
    Arrays.fill(dp, 1);
    
    int maxLength = 1;
    
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        maxLength = Math.max(maxLength, dp[i]);
    }
    
    return maxLength;
}
```

```java
// Approach 2: Dynamic Programming with Binary Search (O(n log n))
public int lengthOfLIS(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    int n = nums.length;
    int[] tails = new int[n];
    int size = 0;
    
    for (int num : nums) {
        int left = 0;
        int right = size;
        
        // Binary search to find the position to insert/replace
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (tails[mid] < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        // Update tails array
        tails[left] = num;
        
        // If we're appending to the end, increase the size
        if (left == size) {
            size++;
        }
    }
    
    return size;
}
```

```java
// Alternative implementation using Java's built-in binary search
public int lengthOfLIS(int[] nums) {
    List<Integer> tails = new ArrayList<>();
    
    for (int num : nums) {
        if (tails.isEmpty() || num > tails.get(tails.size() - 1)) {
            tails.add(num);
        } else {
            int index = Collections.binarySearch(tails, num);
            if (index < 0) {
                index = -(index + 1);
            }
            tails.set(index, num);
        }
    }
    
    return tails.size();
}
``` 