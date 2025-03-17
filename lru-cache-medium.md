# LRU Cache Medium

**Difficulty**: Medium  
**Tags**: Hash Table, Linked List, Design, Doubly-Linked List

## Question
Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the `LRUCache` class:
- `LRUCache(int capacity)` Initialize the LRU cache with positive size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, evict the least recently used key.

The functions `get` and `put` must each run in O(1) average time complexity.

## Example Input/Output
```
Input:
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output:
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation:
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

## Solution Algorithm
To implement an LRU cache with O(1) time complexity for both `get` and `put` operations, we need to use a combination of a hash map and a doubly linked list:

1. Use a hash map to store key-value pairs for O(1) access.
2. Use a doubly linked list to maintain the order of elements based on their usage.
3. When a key is accessed or updated, move it to the front of the linked list.
4. When the cache is full and a new key is added, remove the least recently used key (the one at the end of the linked list).

### Key Components:
- `HashMap<Integer, Node>`: Maps keys to nodes in the doubly linked list.
- `DoublyLinkedList`: Maintains the order of elements based on their usage.
- `head` and `tail`: Dummy nodes to simplify edge cases.

### Operations:
1. `get(key)`:
   - If the key exists, retrieve the node from the hash map.
   - Move the node to the front of the linked list (mark it as recently used).
   - Return the value.
   - If the key doesn't exist, return -1.

2. `put(key, value)`:
   - If the key already exists, update its value and move it to the front of the linked list.
   - If the key doesn't exist:
     - Create a new node with the key-value pair.
     - Add it to the hash map.
     - Add it to the front of the linked list.
     - If the cache exceeds its capacity, remove the least recently used node (the one at the end of the linked list).

## Solution Code
```java
class LRUCache {
    private class Node {
        int key;
        int value;
        Node prev;
        Node next;
        
        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
    
    private int capacity;
    private Map<Integer, Node> cache;
    private Node head; // Dummy head
    private Node tail; // Dummy tail
    
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new HashMap<>();
        
        // Initialize dummy head and tail
        this.head = new Node(0, 0);
        this.tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }
        
        // Get the node from the cache
        Node node = cache.get(key);
        
        // Move the node to the front (mark as recently used)
        removeNode(node);
        addToFront(node);
        
        return node.value;
    }
    
    public void put(int key, int value) {
        // If the key already exists, update its value and move it to the front
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            node.value = value;
            
            removeNode(node);
            addToFront(node);
            return;
        }
        
        // If the cache is full, remove the least recently used item (the one at the end)
        if (cache.size() >= capacity) {
            Node lru = tail.prev;
            removeNode(lru);
            cache.remove(lru.key);
        }
        
        // Add the new node to the front
        Node newNode = new Node(key, value);
        cache.put(key, newNode);
        addToFront(newNode);
    }
    
    private void removeNode(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    
    private void addToFront(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
}
```

```java
// Alternative implementation with more descriptive variable names
class LRUCache {
    private class DLinkedNode {
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;
        
        public DLinkedNode() {}
        
        public DLinkedNode(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
    
    private Map<Integer, DLinkedNode> cache;
    private int capacity;
    private int size;
    private DLinkedNode head, tail; // Dummy head and tail nodes
    
    public LRUCache(int capacity) {
        this.cache = new HashMap<>();
        this.capacity = capacity;
        this.size = 0;
        
        // Initialize dummy head and tail nodes
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            return -1;
        }
        
        // Move the accessed node to the front
        moveToHead(node);
        return node.value;
    }
    
    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        
        if (node == null) {
            // Create a new node
            DLinkedNode newNode = new DLinkedNode(key, value);
            
            // Add to the cache
            cache.put(key, newNode);
            
            // Add to the front of the linked list
            addNode(newNode);
            size++;
            
            // Check if we need to remove the least recently used item
            if (size > capacity) {
                // Remove the tail node (least recently used)
                DLinkedNode tail = removeTail();
                cache.remove(tail.key);
                size--;
            }
        } else {
            // Update the value of the existing node
            node.value = value;
            
            // Move it to the front
            moveToHead(node);
        }
    }
    
    // Add a node right after the head
    private void addNode(DLinkedNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }
    
    // Remove a node from the linked list
    private void removeNode(DLinkedNode node) {
        DLinkedNode prev = node.prev;
        DLinkedNode next = node.next;
        
        prev.next = next;
        next.prev = prev;
    }
    
    // Move a node to the head (mark as most recently used)
    private void moveToHead(DLinkedNode node) {
        removeNode(node);
        addNode(node);
    }
    
    // Remove and return the tail node (least recently used)
    private DLinkedNode removeTail() {
        DLinkedNode res = tail.prev;
        removeNode(res);
        return res;
    }
}
```

```java
// Implementation using Java's built-in LinkedHashMap
import java.util.LinkedHashMap;
import java.util.Map;

class LRUCache extends LinkedHashMap<Integer, Integer> {
    private final int capacity;
    
    public LRUCache(int capacity) {
        // Initial capacity, load factor, and access order
        super(capacity, 0.75f, true);
        this.capacity = capacity;
    }
    
    public int get(int key) {
        return super.getOrDefault(key, -1);
    }
    
    public void put(int key, int value) {
        super.put(key, value);
    }
    
    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        // Return true if the map size exceeds the capacity
        return size() > capacity;
    }
}
``` 