# Trapping Rain Water

**Difficulty**: Hard  
**Tags**: Arrays, Two Pointers, Dynamic Programming

## Question
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

## Example Input/Output
**Input**: [0,1,0,2,1,0,1,3,2,1,2,1]  
**Output**: 6

**Explanation**: The elevation map is shown below. In this case, 6 units of rain water (blue section) are being trapped.
```
     #
 # # ##
#######
```

## Solution Algorithm
1. Use a two-pointer approach (left and right)
2. Keep track of maximum heights seen from left and right
3. The water trapped at any position is determined by the minimum of the maximum heights from both sides minus the height at that position
4. Move the pointer with the smaller maximum height toward the center

## Solution Code
```java
public int trap(int[] height) {
    if (height == null || height.length < 3) {
        return 0;
    }
    
    int left = 0;
    int right = height.length - 1;
    int leftMax = 0;
    int rightMax = 0;
    int water = 0;
    
    while (left < right) {
        // Update maximum heights seen from left and right
        leftMax = Math.max(leftMax, height[left]);
        rightMax = Math.max(rightMax, height[right]);
        
        // Calculate water trapped at current position
        if (leftMax < rightMax) {
            water += leftMax - height[left];
            left++;
        } else {
            water += rightMax - height[right];
            right--;
        }
    }
    
    return water;
} 