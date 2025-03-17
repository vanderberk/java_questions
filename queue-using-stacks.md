# Queue Implementation Using Stacks

**Difficulty**: Medium  
**Tags**: Queues, Stacks, Data Structures

## Question
Implement a queue using two stacks. The implemented queue should support all standard queue operations (push, peek, pop, and empty).

## Example Input/Output
```
MyQueue queue = new MyQueue();
queue.push(1);
queue.push(2);
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

## Solution Algorithm
1. Use two stacks: one for pushing elements (stackPush), the other for popping elements (stackPop)
2. When pushing an element, simply push to stackPush
3. When popping or peeking:
   - If stackPop is empty, transfer all elements from stackPush to stackPop
   - Pop or peek from stackPop
4. The queue is empty when both stacks are empty

## Solution Code
```java
import java.util.Stack;

class MyQueue {
    private Stack<Integer> stackPush;
    private Stack<Integer> stackPop;
    
    public MyQueue() {
        stackPush = new Stack<>();
        stackPop = new Stack<>();
    }
    
    public void push(int x) {
        stackPush.push(x);
    }
    
    public int pop() {
        if (stackPop.empty()) {
            while (!stackPush.empty()) {
                stackPop.push(stackPush.pop());
            }
        }
        
        return stackPop.pop();
    }
    
    public int peek() {
        if (stackPop.empty()) {
            while (!stackPush.empty()) {
                stackPop.push(stackPush.pop());
            }
        }
        
        return stackPop.peek();
    }
    
    public boolean empty() {
        return stackPush.empty() && stackPop.empty();
    }
} 