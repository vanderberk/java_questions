# Min Stack

**Difficulty**: Easy  
**Tags**: Stack, Design

## Question
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

## Example Input/Output
**Input**:  
["MinStack","push","push","push","getMin","pop","top","getMin"]  
[[],[-2],[0],[-3],[],[],[],[]]

**Output**:  
[null,null,null,null,-3,null,0,-2]

**Explanation**:  
MinStack minStack = new MinStack();  
minStack.push(-2);  
minStack.push(0);  
minStack.push(-3);  
minStack.getMin(); // return -3  
minStack.pop();  
minStack.top();    // return 0  
minStack.getMin(); // return -2  

## Solution Algorithm
There are several approaches to solve this problem:

### Approach 1: Using Two Stacks
1. Use one stack to store the elements.
2. Use another stack to keep track of the minimum element at each state.
3. When pushing an element, push it to the main stack. If the element is less than or equal to the current minimum (or if the min stack is empty), push it to the min stack as well.
4. When popping an element, pop from the main stack. If the popped element is equal to the top of the min stack, pop from the min stack as well.
5. To get the minimum element, return the top of the min stack.

### Approach 2: Using a Single Stack with Pairs
1. Use a single stack to store pairs of (value, current minimum).
2. When pushing an element, calculate the new minimum (min of the current element and the previous minimum) and push the pair (value, new minimum).
3. When popping, just pop the pair from the stack.
4. To get the minimum element, return the minimum value from the top pair.

## Solution Code
```java
// Approach 1: Using Two Stacks
class MinStack {
    private Stack<Integer> stack;
    private Stack<Integer> minStack;

    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();
    }
    
    public void push(int val) {
        stack.push(val);
        
        // If minStack is empty or val is less than or equal to the current minimum,
        // push val to minStack
        if (minStack.isEmpty() || val <= minStack.peek()) {
            minStack.push(val);
        }
    }
    
    public void pop() {
        // If the popped element is the current minimum, pop from minStack as well
        if (stack.peek().equals(minStack.peek())) {
            minStack.pop();
        }
        
        stack.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return minStack.peek();
    }
}
```

```java
// Approach 2: Using a Single Stack with Pairs
class MinStack {
    private Stack<int[]> stack;

    public MinStack() {
        stack = new Stack<>();
    }
    
    public void push(int val) {
        // If stack is empty, the minimum is val
        // Otherwise, the minimum is the minimum of val and the current minimum
        int min = stack.isEmpty() ? val : Math.min(val, stack.peek()[1]);
        stack.push(new int[]{val, min});
    }
    
    public void pop() {
        stack.pop();
    }
    
    public int top() {
        return stack.peek()[0];
    }
    
    public int getMin() {
        return stack.peek()[1];
    }
}
``` 