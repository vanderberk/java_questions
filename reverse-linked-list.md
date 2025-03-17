# Reverse Linked List

**Difficulty**: Easy  
**Tags**: Linked List, Recursion

## Question
Given the head of a singly linked list, reverse the list, and return the reversed list.

## Example Input/Output
**Input**: head = [1,2,3,4,5]  
**Output**: [5,4,3,2,1]

**Input**: head = [1,2]  
**Output**: [2,1]

**Input**: head = []  
**Output**: []

## Solution Algorithm
There are two main approaches to reverse a linked list:

### Iterative Approach
1. Initialize three pointers: `prev` as null, `current` as head, and `next` as null.
2. Iterate through the list:
   - Store the next node: `next = current.next`
   - Reverse the current node's pointer: `current.next = prev`
   - Move `prev` and `current` one step forward: `prev = current`, `current = next`
3. Return `prev` as the new head of the reversed list.

### Recursive Approach
1. Base case: If the list is empty or has only one node, return the head.
2. Recursively call the function for the rest of the list (head.next).
3. Reverse the link between the current node and the next node:
   - `head.next.next = head`
   - `head.next = null`
4. Return the new head of the reversed list.

## Solution Code
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
public class Solution {
    // Iterative approach
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        ListNode next = null;
        
        while (current != null) {
            next = current.next;    // Store next node
            current.next = prev;    // Reverse the link
            prev = current;         // Move prev one step forward
            current = next;         // Move current one step forward
        }
        
        return prev;  // prev is the new head of the reversed list
    }
}
```

```java
// Recursive approach
public ListNode reverseList(ListNode head) {
    // Base case: empty list or list with only one node
    if (head == null || head.next == null) {
        return head;
    }
    
    // Recursively reverse the rest of the list
    ListNode newHead = reverseList(head.next);
    
    // Reverse the link between current node and next node
    head.next.next = head;
    head.next = null;
    
    return newHead;  // Return the new head of the reversed list
}
```

```java
// In-place reversal with a single pass (alternative iterative approach)
public ListNode reverseList(ListNode head) {
    if (head == null) {
        return null;
    }
    
    ListNode newHead = head;
    ListNode nextNode = head.next;
    newHead.next = null;
    
    while (nextNode != null) {
        ListNode temp = nextNode.next;
        nextNode.next = newHead;
        newHead = nextNode;
        nextNode = temp;
    }
    
    return newHead;
}
``` 