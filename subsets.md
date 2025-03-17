# Subsets

**Difficulty**: Medium  
**Tags**: Array, Backtracking, Bit Manipulation

## Question
Given an integer array `nums` of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

## Example Input/Output
**Input**: nums = [1,2,3]  
**Output**: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

**Input**: nums = [0]  
**Output**: [[],[0]]

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Backtracking
1. Create a recursive function that takes the input array, the current index, the current subset, and a list to store all subsets.
2. For each index, we have two choices: include the element at the current index or exclude it.
3. Base case: If the current index is equal to the length of the input array, add the current subset to the list of subsets.
4. Recursively call the function for both choices (include and exclude).
5. Return the list of all subsets.

### Approach 2: Iterative
1. Start with an empty subset.
2. For each element in the input array, add it to each existing subset to create new subsets.
3. Add these new subsets to the list of all subsets.
4. Return the list of all subsets.

### Approach 3: Bit Manipulation
1. For an array of length n, there are 2^n possible subsets.
2. Each subset can be represented as a binary number of length n, where a 1 at position i indicates that the element at index i is included in the subset.
3. Generate all binary numbers from 0 to 2^n - 1 and create the corresponding subsets.
4. Return the list of all subsets.

## Solution Code
```java
// Approach 1: Backtracking
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] nums, int index, List<Integer> currentSubset, List<List<Integer>> result) {
    // Base case: If we've processed all elements
    if (index == nums.length) {
        result.add(new ArrayList<>(currentSubset));
        return;
    }
    
    // Choice 1: Exclude the current element
    backtrack(nums, index + 1, currentSubset, result);
    
    // Choice 2: Include the current element
    currentSubset.add(nums[index]);
    backtrack(nums, index + 1, currentSubset, result);
    
    // Backtrack: Remove the last element to restore the original subset
    currentSubset.remove(currentSubset.size() - 1);
}
```

```java
// Approach 2: Iterative
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    result.add(new ArrayList<>()); // Start with an empty subset
    
    for (int num : nums) {
        int size = result.size();
        for (int i = 0; i < size; i++) {
            List<Integer> newSubset = new ArrayList<>(result.get(i));
            newSubset.add(num);
            result.add(newSubset);
        }
    }
    
    return result;
}
```

```java
// Approach 3: Bit Manipulation
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    int n = nums.length;
    int totalSubsets = 1 << n; // 2^n
    
    for (int i = 0; i < totalSubsets; i++) {
        List<Integer> subset = new ArrayList<>();
        for (int j = 0; j < n; j++) {
            // Check if the jth bit is set in i
            if ((i & (1 << j)) != 0) {
                subset.add(nums[j]);
            }
        }
        result.add(subset);
    }
    
    return result;
}
``` 