# Huffman Coding

**Difficulty**: Medium  
**Tags**: Tree, Heap, Greedy, Compression

## Question
Implement Huffman coding, a lossless data compression algorithm. The algorithm assigns variable-length codes to input characters, with shorter codes assigned to more frequent characters, resulting in prefix codes.

Given a string, implement the Huffman coding algorithm to compress it and then decompress it back to the original string. Your implementation should:

1. Build a frequency table for each character in the input string.
2. Build a Huffman tree using a priority queue.
3. Generate Huffman codes for each character.
4. Encode the input string using the generated codes.
5. Decode the encoded string back to the original string.

## Example Input/Output
**Input**: "AAAAABBBCCD"  
**Output**:  
Frequency Table: {A=5, B=3, C=2, D=1}  
Huffman Codes: {A=0, B=10, C=110, D=111}  
Encoded: 00000101010110110111  
Decoded: "AAAAABBBCCD"  

## Solution Algorithm
The Huffman coding algorithm consists of the following steps:

1. **Build a frequency table**:
   - Count the frequency of each character in the input string.

2. **Build a Huffman tree**:
   - Create a leaf node for each character and add it to a priority queue, with priority based on the frequency.
   - While there is more than one node in the queue:
     - Remove the two nodes with the lowest frequency.
     - Create a new internal node with these two nodes as children and a frequency equal to the sum of their frequencies.
     - Add the new node to the queue.
   - The remaining node is the root of the Huffman tree.

3. **Generate Huffman codes**:
   - Traverse the Huffman tree to assign codes to each character.
   - Going left adds a '0' to the code, going right adds a '1'.

4. **Encode the input**:
   - Replace each character in the input with its Huffman code.

5. **Decode the encoded string**:
   - Use the Huffman tree to decode the encoded string back to the original.
   - Start from the root and traverse the tree based on the bits in the encoded string.
   - When a leaf node is reached, output the character and restart from the root.

## Solution Code
```java
import java.util.*;

public class HuffmanCoding {
    // Node class for Huffman tree
    private static class Node implements Comparable<Node> {
        char character;
        int frequency;
        Node left, right;
        
        public Node(char character, int frequency) {
            this.character = character;
            this.frequency = frequency;
        }
        
        public Node(int frequency, Node left, Node right) {
            this.frequency = frequency;
            this.left = left;
            this.right = right;
        }
        
        @Override
        public int compareTo(Node other) {
            return this.frequency - other.frequency;
        }
        
        public boolean isLeaf() {
            return left == null && right == null;
        }
    }
    
    // Build frequency table
    private Map<Character, Integer> buildFrequencyTable(String text) {
        Map<Character, Integer> freqTable = new HashMap<>();
        for (char c : text.toCharArray()) {
            freqTable.put(c, freqTable.getOrDefault(c, 0) + 1);
        }
        return freqTable;
    }
    
    // Build Huffman tree
    private Node buildHuffmanTree(Map<Character, Integer> freqTable) {
        PriorityQueue<Node> pq = new PriorityQueue<>();
        
        // Create leaf nodes for each character
        for (Map.Entry<Character, Integer> entry : freqTable.entrySet()) {
            pq.add(new Node(entry.getKey(), entry.getValue()));
        }
        
        // Build the tree by combining nodes
        while (pq.size() > 1) {
            Node left = pq.poll();
            Node right = pq.poll();
            
            Node parent = new Node(left.frequency + right.frequency, left, right);
            pq.add(parent);
        }
        
        return pq.poll(); // Root of the Huffman tree
    }
    
    // Generate Huffman codes
    private Map<Character, String> generateCodes(Node root) {
        Map<Character, String> codes = new HashMap<>();
        generateCodesRecursive(root, "", codes);
        return codes;
    }
    
    private void generateCodesRecursive(Node node, String code, Map<Character, String> codes) {
        if (node == null) return;
        
        if (node.isLeaf()) {
            codes.put(node.character, code.isEmpty() ? "0" : code);
            return;
        }
        
        generateCodesRecursive(node.left, code + "0", codes);
        generateCodesRecursive(node.right, code + "1", codes);
    }
    
    // Encode the input string
    private String encode(String text, Map<Character, String> codes) {
        StringBuilder encoded = new StringBuilder();
        for (char c : text.toCharArray()) {
            encoded.append(codes.get(c));
        }
        return encoded.toString();
    }
    
    // Decode the encoded string
    private String decode(String encoded, Node root) {
        StringBuilder decoded = new StringBuilder();
        Node current = root;
        
        for (char bit : encoded.toCharArray()) {
            if (bit == '0') {
                current = current.left;
            } else {
                current = current.right;
            }
            
            if (current.isLeaf()) {
                decoded.append(current.character);
                current = root;
            }
        }
        
        return decoded.toString();
    }
    
    // Main method to compress and decompress
    public void compressAndDecompress(String text) {
        // Build frequency table
        Map<Character, Integer> freqTable = buildFrequencyTable(text);
        System.out.println("Frequency Table: " + freqTable);
        
        // Build Huffman tree
        Node root = buildHuffmanTree(freqTable);
        
        // Generate Huffman codes
        Map<Character, String> codes = generateCodes(root);
        System.out.println("Huffman Codes: " + codes);
        
        // Encode the input
        String encoded = encode(text, codes);
        System.out.println("Encoded: " + encoded);
        
        // Decode the encoded string
        String decoded = decode(encoded, root);
        System.out.println("Decoded: " + decoded);
    }
    
    public static void main(String[] args) {
        HuffmanCoding huffman = new HuffmanCoding();
        huffman.compressAndDecompress("AAAAABBBCCD");
    }
}
```

```java
// Alternative implementation with more detailed comments
import java.util.*;

public class HuffmanCoding {
    // Node class for Huffman tree
    private static class HuffmanNode implements Comparable<HuffmanNode> {
        char character;
        int frequency;
        HuffmanNode left, right;
        boolean isLeaf;
        
        // Constructor for leaf nodes (with characters)
        public HuffmanNode(char character, int frequency) {
            this.character = character;
            this.frequency = frequency;
            this.isLeaf = true;
        }
        
        // Constructor for internal nodes (without characters)
        public HuffmanNode(int frequency, HuffmanNode left, HuffmanNode right) {
            this.frequency = frequency;
            this.left = left;
            this.right = right;
            this.isLeaf = false;
        }
        
        @Override
        public int compareTo(HuffmanNode other) {
            // Compare nodes based on frequency for priority queue
            return this.frequency - other.frequency;
        }
    }
    
    // Compress a string using Huffman coding
    public Map<String, Object> compress(String input) {
        if (input == null || input.isEmpty()) {
            return Collections.emptyMap();
        }
        
        // Step 1: Build frequency table
        Map<Character, Integer> frequencyMap = new HashMap<>();
        for (char c : input.toCharArray()) {
            frequencyMap.put(c, frequencyMap.getOrDefault(c, 0) + 1);
        }
        
        // Step 2: Build Huffman tree
        HuffmanNode root = buildTree(frequencyMap);
        
        // Step 3: Generate codes for each character
        Map<Character, String> codeMap = new HashMap<>();
        generateCodes(root, "", codeMap);
        
        // Step 4: Encode the input string
        StringBuilder encodedString = new StringBuilder();
        for (char c : input.toCharArray()) {
            encodedString.append(codeMap.get(c));
        }
        
        // Return all necessary information for decompression
        Map<String, Object> result = new HashMap<>();
        result.put("encodedString", encodedString.toString());
        result.put("huffmanTree", root);
        result.put("codeMap", codeMap);
        result.put("frequencyMap", frequencyMap);
        
        return result;
    }
    
    // Build the Huffman tree from frequency map
    private HuffmanNode buildTree(Map<Character, Integer> frequencyMap) {
        // Create a priority queue to hold nodes
        PriorityQueue<HuffmanNode> pq = new PriorityQueue<>();
        
        // Add leaf nodes for each character
        for (Map.Entry<Character, Integer> entry : frequencyMap.entrySet()) {
            pq.add(new HuffmanNode(entry.getKey(), entry.getValue()));
        }
        
        // Special case: if there's only one character
        if (pq.size() == 1) {
            HuffmanNode singleNode = pq.poll();
            return new HuffmanNode(singleNode.frequency, singleNode, null);
        }
        
        // Build the tree by combining nodes
        while (pq.size() > 1) {
            HuffmanNode left = pq.poll();
            HuffmanNode right = pq.poll();
            
            // Create a new internal node with these two as children
            HuffmanNode parent = new HuffmanNode(left.frequency + right.frequency, left, right);
            pq.add(parent);
        }
        
        // Return the root of the Huffman tree
        return pq.poll();
    }
    
    // Generate Huffman codes by traversing the tree
    private void generateCodes(HuffmanNode node, String code, Map<Character, String> codeMap) {
        if (node == null) return;
        
        // If it's a leaf node, add its code to the map
        if (node.isLeaf) {
            // Handle the special case of a tree with only one node
            codeMap.put(node.character, code.isEmpty() ? "0" : code);
            return;
        }
        
        // Traverse left (add '0' to code)
        generateCodes(node.left, code + "0", codeMap);
        
        // Traverse right (add '1' to code)
        generateCodes(node.right, code + "1", codeMap);
    }
    
    // Decompress an encoded string using the Huffman tree
    public String decompress(String encodedString, HuffmanNode root) {
        if (encodedString == null || encodedString.isEmpty()) {
            return "";
        }
        
        StringBuilder decodedString = new StringBuilder();
        HuffmanNode current = root;
        
        // Traverse the tree based on the bits in the encoded string
        for (char bit : encodedString.toCharArray()) {
            // Navigate left or right based on the bit
            if (bit == '0') {
                current = current.left;
            } else if (bit == '1') {
                current = current.right;
            }
            
            // If we reach a leaf node, add its character to the result
            if (current.isLeaf) {
                decodedString.append(current.character);
                current = root; // Start again from the root
            }
        }
        
        return decodedString.toString();
    }
} 