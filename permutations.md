# Permutations

**Difficulty**: Medium  
**Tags**: Array, Backtracking, Recursion

## Question
Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.

## Example Input/Output
**Input**: nums = [1,2,3]  
**Output**: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Input**: nums = [0,1]  
**Output**: [[0,1],[1,0]]

**Input**: nums = [1]  
**Output**: [[1]]

## Solution Algorithm
We can solve this problem using backtracking:

1. Create a recursive function that takes the input array, a current permutation, and a list to store all permutations.
2. Base case: If the current permutation has the same length as the input array, add it to the list of permutations.
3. For each element in the input array:
   - If the element is not already in the current permutation, add it to the current permutation.
   - Recursively call the function with the updated current permutation.
   - Remove the last element from the current permutation (backtrack) to try other possibilities.
4. Return the list of all permutations.

## Solution Code
```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] nums, List<Integer> currentPermutation, List<List<Integer>> result) {
    // Base case: If the current permutation has the same length as the input array
    if (currentPermutation.size() == nums.length) {
        result.add(new ArrayList<>(currentPermutation));
        return;
    }
    
    // Try each number
    for (int num : nums) {
        // Skip if the number is already in the current permutation
        if (currentPermutation.contains(num)) {
            continue;
        }
        
        // Add the number to the current permutation
        currentPermutation.add(num);
        
        // Recursively generate permutations with the updated current permutation
        backtrack(nums, currentPermutation, result);
        
        // Remove the last number (backtrack) to try other possibilities
        currentPermutation.remove(currentPermutation.size() - 1);
    }
}
```

// Alternative implementation using a boolean array to track used elements
```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    boolean[] used = new boolean[nums.length];
    backtrack(nums, used, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] nums, boolean[] used, List<Integer> currentPermutation, List<List<Integer>> result) {
    // Base case: If the current permutation has the same length as the input array
    if (currentPermutation.size() == nums.length) {
        result.add(new ArrayList<>(currentPermutation));
        return;
    }
    
    // Try each number
    for (int i = 0; i < nums.length; i++) {
        // Skip if the number is already used
        if (used[i]) {
            continue;
        }
        
        // Mark the number as used
        used[i] = true;
        
        // Add the number to the current permutation
        currentPermutation.add(nums[i]);
        
        // Recursively generate permutations with the updated current permutation
        backtrack(nums, used, currentPermutation, result);
        
        // Remove the last number (backtrack) to try other possibilities
        currentPermutation.remove(currentPermutation.size() - 1);
        
        // Mark the number as unused
        used[i] = false;
    }
}
```

// Alternative implementation using swapping elements in the array
```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    permuteHelper(nums, 0, result);
    return result;
}

private void permuteHelper(int[] nums, int start, List<List<Integer>> result) {
    // Base case: If we've processed all elements
    if (start == nums.length) {
        List<Integer> permutation = new ArrayList<>();
        for (int num : nums) {
            permutation.add(num);
        }
        result.add(permutation);
        return;
    }
    
    // Try each element as the next element in the permutation
    for (int i = start; i < nums.length; i++) {
        // Swap the current element with the element at index start
        swap(nums, start, i);
        
        // Recursively generate permutations for the remaining elements
        permuteHelper(nums, start + 1, result);
        
        // Swap back (backtrack) to restore the original array
        swap(nums, start, i);
    }
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
``` 