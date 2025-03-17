# Pascal's Triangle

**Difficulty**: Easy  
**Tags**: Array, Dynamic Programming

## Question
Given an integer `numRows`, return the first `numRows` of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it.

## Example Input/Output
**Input**: numRows = 5  
**Output**: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]

**Input**: numRows = 1  
**Output**: [[1]]

## Solution Algorithm
Pascal's triangle can be generated using the following approach:

1. Create an empty list to store the rows of Pascal's triangle.
2. For each row from 0 to numRows-1:
   - Create a new list for the current row.
   - The first and last elements of each row are always 1.
   - For the middle elements (if any), calculate each value as the sum of the two elements directly above it from the previous row.
   - Add the current row to the result list.
3. Return the result list.

## Solution Code
```java
public List<List<Integer>> generate(int numRows) {
    List<List<Integer>> triangle = new ArrayList<>();
    
    // Base case
    if (numRows <= 0) {
        return triangle;
    }
    
    // First row is always [1]
    triangle.add(new ArrayList<>());
    triangle.get(0).add(1);
    
    // Generate the rest of the rows
    for (int rowNum = 1; rowNum < numRows; rowNum++) {
        List<Integer> row = new ArrayList<>();
        List<Integer> prevRow = triangle.get(rowNum - 1);
        
        // First element is always 1
        row.add(1);
        
        // Middle elements are sum of elements above
        for (int j = 1; j < rowNum; j++) {
            row.add(prevRow.get(j - 1) + prevRow.get(j));
        }
        
        // Last element is always 1
        row.add(1);
        
        triangle.add(row);
    }
    
    return triangle;
}
```

```java
// Alternative implementation using a more functional approach
public List<List<Integer>> generate(int numRows) {
    List<List<Integer>> triangle = new ArrayList<>();
    
    if (numRows <= 0) {
        return triangle;
    }
    
    // Initialize with the first row
    triangle.add(Collections.singletonList(1));
    
    for (int i = 1; i < numRows; i++) {
        List<Integer> prevRow = triangle.get(i - 1);
        List<Integer> currentRow = new ArrayList<>();
        
        currentRow.add(1); // First element
        
        // Calculate middle elements
        for (int j = 0; j < prevRow.size() - 1; j++) {
            currentRow.add(prevRow.get(j) + prevRow.get(j + 1));
        }
        
        currentRow.add(1); // Last element
        triangle.add(currentRow);
    }
    
    return triangle;
}
``` 