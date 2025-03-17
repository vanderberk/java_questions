# LRU Cache

**Difficulty**: Hard  
**Tags**: Hash Table, Linked List, Design

## Question
Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations:

- get(key): Get the value of the key if the key exists in the cache, otherwise return -1.
- put(key, value): Set or insert the value if the key is not already present. When the cache reaches its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a positive capacity.

## Example Input/Output
```
LRUCache cache = new LRUCache(2); // capacity = 2
cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

## Solution Algorithm
1. Use a doubly linked list to maintain the order of elements based on their usage frequency
2. Use a hash map that maps keys to nodes in the linked list for O(1) access
3. When get() is called, move the accessed node to the front of the list (most recently used)
4. When put() is called:
   - If the key exists, update the value and move the node to the front
   - If the key doesn't exist, create a new node and add it to the front
   - If the capacity is full, remove the least recently used node (from the end of the list)

## Solution Code
```java
import java.util.HashMap;
import java.util.Map;

class LRUCache {
    class Node {
        int key;
        int value;
        Node prev;
        Node next;
        
        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
    
    private Map<Integer, Node> cache;
    private int capacity;
    private Node head; // Most recently used
    private Node tail; // Least recently used
    
    public LRUCache(int capacity) {
        this.capacity = capacity;
        cache = new HashMap<>();
        
        // Initialize dummy head and tail nodes
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }
        
        // Move accessed node to front
        Node node = cache.get(key);
        removeNode(node);
        addToFront(node);
        
        return node.value;
    }
    
    public void put(int key, int value) {
        // If key exists, remove it
        if (cache.containsKey(key)) {
            removeNode(cache.get(key));
        }
        
        // Create new node
        Node newNode = new Node(key, value);
        
        // Add to cache and to front of list
        cache.put(key, newNode);
        addToFront(newNode);
        
        // If capacity exceeded, remove LRU (tail)
        if (cache.size() > capacity) {
            Node lru = tail.prev;
            removeNode(lru);
            cache.remove(lru.key);
        }
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