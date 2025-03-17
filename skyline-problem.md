# The Skyline Problem

**Difficulty**: Hard  
**Tags**: Array, Divide and Conquer, Heap (Priority Queue), Segment Tree, Line Sweep, Ordered Set

## Question
A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Given the locations and heights of all the buildings, return the skyline formed by these buildings collectively.

The geometric information of each building is given in the array `buildings` where `buildings[i] = [lefti, righti, heighti]`:
- `lefti` is the x coordinate of the left edge of the ith building.
- `righti` is the x coordinate of the right edge of the ith building.
- `heighti` is the height of the ith building.

You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

The skyline should be represented as a list of "key points" sorted by their x-coordinate in the form `[[x1,y1],[x2,y2],...]`. Each key point is the left endpoint of some horizontal segment in the skyline except the last point in the list, which always has a y-coordinate 0 and is used to mark the skyline's termination where the rightmost building ends. Any ground between the leftmost and rightmost buildings should be part of the skyline's contour.

**Note**: There must be no consecutive horizontal lines of equal height in the output skyline. For instance, `[...[2 3], [4 5], [7 5], [11 5], [12 7]...]` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: `[...[2 3], [4 5], [12 7], ...]`

## Example Input/Output
**Example 1:**
```
Input: buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]
Output: [[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]
```

**Example 2:**
```
Input: buildings = [[0,2,3],[2,5,3]]
Output: [[0,3],[5,0]]
```

## Solution Algorithm
This problem can be solved using several approaches:

### Approach 1: Divide and Conquer
1. Divide the buildings into two halves.
2. Recursively compute the skyline for each half.
3. Merge the two skylines similar to the merge step in merge sort.

### Approach 2: Priority Queue (Heap)
1. Extract all the critical points (start and end of each building).
2. Sort these points by x-coordinate.
3. Use a max heap to keep track of the current heights.
4. For each critical point:
   - If it's a start point, add the height to the heap.
   - If it's an end point, remove the height from the heap.
   - The current maximum height in the heap is the skyline height at this point.

### Approach 3: Line Sweep
1. Create events for each building's start and end points.
2. Sort the events by x-coordinate.
3. Process the events in order, keeping track of the current maximum height.
4. When the maximum height changes, add a new point to the skyline.

## Solution Code
```java
// Approach 1: Divide and Conquer
public List<List<Integer>> getSkyline(int[][] buildings) {
    if (buildings.length == 0) {
        return new ArrayList<>();
    }
    return divideAndConquer(buildings, 0, buildings.length - 1);
}

private List<List<Integer>> divideAndConquer(int[][] buildings, int start, int end) {
    if (start == end) {
        List<List<Integer>> result = new ArrayList<>();
        result.add(Arrays.asList(buildings[start][0], buildings[start][2]));
        result.add(Arrays.asList(buildings[start][1], 0));
        return result;
    }
    
    int mid = start + (end - start) / 2;
    List<List<Integer>> left = divideAndConquer(buildings, start, mid);
    List<List<Integer>> right = divideAndConquer(buildings, mid + 1, end);
    
    return merge(left, right);
}

private List<List<Integer>> merge(List<List<Integer>> left, List<List<Integer>> right) {
    List<List<Integer>> result = new ArrayList<>();
    int leftHeight = 0, rightHeight = 0;
    int i = 0, j = 0;
    
    while (i < left.size() && j < right.size()) {
        int x;
        int h1 = left.get(i).get(1);
        int h2 = right.get(j).get(1);
        
        // Choose the point with smaller x-coordinate
        if (left.get(i).get(0) < right.get(j).get(0)) {
            x = left.get(i).get(0);
            leftHeight = h1;
            i++;
        } else {
            x = right.get(j).get(0);
            rightHeight = h2;
            j++;
        }
        
        // Determine the current height
        int height = Math.max(leftHeight, rightHeight);
        
        // Add to result if the height changes
        if (result.isEmpty() || height != result.get(result.size() - 1).get(1)) {
            result.add(Arrays.asList(x, height));
        }
    }
    
    // Add remaining points
    while (i < left.size()) {
        result.add(left.get(i++));
    }
    
    while (j < right.size()) {
        result.add(right.get(j++));
    }
    
    return result;
}
```

```java
// Approach 2: Priority Queue (Heap)
public List<List<Integer>> getSkyline(int[][] buildings) {
    List<List<Integer>> result = new ArrayList<>();
    
    // Create critical points
    List<int[]> points = new ArrayList<>();
    for (int[] building : buildings) {
        // Start point with negative height to ensure start points are processed before end points
        points.add(new int[]{building[0], -building[2]});
        // End point with positive height
        points.add(new int[]{building[1], building[2]});
    }
    
    // Sort points by x-coordinate, then by height (start points before end points)
    Collections.sort(points, (a, b) -> {
        if (a[0] != b[0]) {
            return a[0] - b[0];
        } else {
            return a[1] - b[1];
        }
    });
    
    // Use a max heap to keep track of heights
    PriorityQueue<Integer> heap = new PriorityQueue<>((a, b) -> b - a);
    heap.offer(0); // Add ground level
    
    int prevMaxHeight = 0;
    
    for (int[] point : points) {
        int x = point[0];
        int h = point[1];
        
        if (h < 0) {
            // Start point, add height to heap
            heap.offer(-h);
        } else {
            // End point, remove height from heap
            heap.remove(h);
        }
        
        // Current max height
        int currMaxHeight = heap.peek();
        
        // If max height changes, add to result
        if (prevMaxHeight != currMaxHeight) {
            result.add(Arrays.asList(x, currMaxHeight));
            prevMaxHeight = currMaxHeight;
        }
    }
    
    return result;
}
```

```java
// Approach 3: TreeMap for efficient removal
public List<List<Integer>> getSkyline(int[][] buildings) {
    List<List<Integer>> result = new ArrayList<>();
    
    // Create critical points
    List<int[]> points = new ArrayList<>();
    for (int[] building : buildings) {
        points.add(new int[]{building[0], -building[2]});
        points.add(new int[]{building[1], building[2]});
    }
    
    // Sort points
    Collections.sort(points, (a, b) -> {
        if (a[0] != b[0]) {
            return a[0] - b[0];
        } else {
            return a[1] - b[1];
        }
    });
    
    // Use TreeMap to efficiently keep track of heights and their frequencies
    TreeMap<Integer, Integer> heightMap = new TreeMap<>(Collections.reverseOrder());
    heightMap.put(0, 1); // Add ground level
    
    int prevMaxHeight = 0;
    
    for (int[] point : points) {
        int x = point[0];
        int h = point[1];
        
        if (h < 0) {
            // Start point
            h = -h;
            heightMap.put(h, heightMap.getOrDefault(h, 0) + 1);
        } else {
            // End point
            heightMap.put(h, heightMap.get(h) - 1);
            if (heightMap.get(h) == 0) {
                heightMap.remove(h);
            }
        }
        
        // Current max height
        int currMaxHeight = heightMap.firstKey();
        
        // If max height changes, add to result
        if (prevMaxHeight != currMaxHeight) {
            result.add(Arrays.asList(x, currMaxHeight));
            prevMaxHeight = currMaxHeight;
        }
    }
    
    return result;
}
``` 