# Rotate Image

**Difficulty**: Medium  
**Tags**: Arrays, Matrix, Mathematics

## Question
Given an n x n 2D matrix representing an image, rotate the image by 90 degrees clockwise in-place.

## Example Input/Output
**Input**:
```
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
]
```

**Output**:
```
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

## Solution Algorithm
1. Transpose the matrix (swap rows with columns)
2. Reverse each row of the transposed matrix

## Solution Code
```java
public void rotate(int[][] matrix) {
    int n = matrix.length;
    
    // Transpose the matrix
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    
    // Reverse each row
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n / 2; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[i][n - 1 - j];
            matrix[i][n - 1 - j] = temp;
        }
    }
} 