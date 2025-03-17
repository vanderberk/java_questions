# Container With Most Water

**Difficulty**: Medium  
**Tags**: Array, Two Pointers, Greedy

## Question
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i`th line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

**Notice** that you may not slant the container.

## Example Input/Output
**Input**: height = [1,8,6,2,5,4,8,3,7]  
**Output**: 49  
**Explanation**: The maximum area is obtained by choosing the 2nd and 8th lines, with heights 8 and 7. The area of this container is min(8, 7) * (8 - 1) = 7 * 7 = 49.

**Input**: height = [1,1]  
**Output**: 1  
**Explanation**: The maximum area is obtained by choosing the 1st and 2nd lines, with heights 1 and 1. The area of this container is min(1, 1) * (2 - 1) = 1 * 1 = 1.

## Solution Algorithm
We can solve this problem using the two-pointer technique:

1. Initialize two pointers, `left` at the beginning of the array and `right` at the end of the array.
2. Calculate the area between the lines at `left` and `right` as `min(height[left], height[right]) * (right - left)`.
3. Update the maximum area if the current area is larger.
4. Move the pointer that points to the shorter line inward (because moving the pointer with the taller line will only decrease the area).
5. Repeat steps 2-4 until the pointers meet.
6. Return the maximum area.

The intuition behind this approach is that the area is limited by the shorter line, so we always move the shorter line's pointer inward to potentially find a taller line that could increase the area.

## Solution Code
```java
public int maxArea(int[] height) {
    int maxArea = 0;
    int left = 0;
    int right = height.length - 1;
    
    while (left < right) {
        // Calculate the width of the container
        int width = right - left;
        
        // Calculate the height of the container (limited by the shorter line)
        int h = Math.min(height[left], height[right]);
        
        // Calculate the area and update maxArea if necessary
        int area = width * h;
        maxArea = Math.max(maxArea, area);
        
        // Move the pointer of the shorter line inward
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxArea;
}
```

// Alternative implementation with cleaner code
```java
public int maxArea(int[] height) {
    int maxArea = 0;
    int left = 0;
    int right = height.length - 1;
    
    while (left < right) {
        // Calculate the current area
        int area = Math.min(height[left], height[right]) * (right - left);
        maxArea = Math.max(maxArea, area);
        
        // Move the pointer of the shorter line inward
        if (height[left] <= height[right]) {
            left++;
        } else {
            right--;
        }
        
        // Optimization: Skip lines that are shorter than the previous line
        // This doesn't change the asymptotic time complexity but can speed up the solution
        while (left < right && height[left] <= height[left - 1]) {
            left++;
        }
        while (left < right && height[right] <= height[right + 1]) {
            right--;
        }
    }
    
    return maxArea;
}
``` 