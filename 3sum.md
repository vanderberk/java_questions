# 3Sum

**Difficulty**: Medium  
**Tags**: Array, Two Pointers, Sorting

## Question
Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

## Example Input/Output
**Input**: nums = [-1,0,1,2,-1,-4]  
**Output**: [[-1,-1,2],[-1,0,1]]  
**Explanation**:  
nums[0] + nums[2] + nums[4] = (-1) + 1 + (-1) = -1 + 1 - 1 = 0.  
nums[1] + nums[2] + nums[5] = 0 + 1 + (-4) = 1 - 4 = -3.  
nums[0] + nums[3] + nums[5] = (-1) + 2 + (-4) = -1 + 2 - 4 = -3.  
The only valid triplets are [-1,-1,2] and [-1,0,1].

**Input**: nums = [0,1,1]  
**Output**: []  
**Explanation**: The only possible triplet doesn't sum up to 0.

**Input**: nums = [0,0,0]  
**Output**: [[0,0,0]]  
**Explanation**: The only possible triplet sums up to 0.

## Solution Algorithm
1. Sort the input array to make it easier to handle duplicates and to use the two-pointer technique.
2. Iterate through the array with a pointer `i`:
   - Skip duplicates for `i` to avoid duplicate triplets
   - For each `i`, use two pointers (`left` and `right`) to find pairs that sum to `-nums[i]`
   - `left` starts at `i+1` and `right` starts at the end of the array
   - If the sum is less than the target, increment `left`
   - If the sum is greater than the target, decrement `right`
   - If the sum equals the target, add the triplet to the result and move both pointers
   - Skip duplicates for `left` and `right` to avoid duplicate triplets
3. Return the list of triplets

## Solution Code
```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    
    // Edge cases
    if (nums == null || nums.length < 3) {
        return result;
    }
    
    // Sort the array to handle duplicates and use two pointers
    Arrays.sort(nums);
    
    for (int i = 0; i < nums.length - 2; i++) {
        // Skip duplicates for i
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        
        int target = -nums[i];
        int left = i + 1;
        int right = nums.length - 1;
        
        while (left < right) {
            int sum = nums[left] + nums[right];
            
            if (sum < target) {
                left++;
            } else if (sum > target) {
                right--;
            } else {
                // Found a triplet
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                
                // Skip duplicates for left and right
                while (left < right && nums[left] == nums[left + 1]) {
                    left++;
                }
                while (left < right && nums[right] == nums[right - 1]) {
                    right--;
                }
                
                // Move both pointers
                left++;
                right--;
            }
        }
    }
    
    return result;
}
```

// Alternative implementation with cleaner code for handling duplicates
```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    
    if (nums == null || nums.length < 3) {
        return result;
    }
    
    Arrays.sort(nums);
    int n = nums.length;
    
    for (int i = 0; i < n - 2; i++) {
        // Skip duplicates for i
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        
        // If the smallest possible sum is greater than 0, break
        if (nums[i] + nums[i + 1] + nums[i + 2] > 0) {
            break;
        }
        
        // If the largest possible sum is less than 0, continue
        if (nums[i] + nums[n - 2] + nums[n - 1] < 0) {
            continue;
        }
        
        int left = i + 1;
        int right = n - 1;
        
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            
            if (sum < 0) {
                left++;
            } else if (sum > 0) {
                right--;
            } else {
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                
                // Move to the next different elements
                int leftVal = nums[left];
                while (left < right && nums[left] == leftVal) {
                    left++;
                }
                
                int rightVal = nums[right];
                while (left < right && nums[right] == rightVal) {
                    right--;
                }
            }
        }
    }
    
    return result;
}
``` 