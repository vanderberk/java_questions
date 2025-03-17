# Word Search II

**Difficulty**: Hard  
**Tags**: Array, Backtracking, Trie, Matrix

## Question
Given an `m x n` board of characters and a list of strings `words`, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

## Example Input/Output
**Example 1:**
```
Input: 
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
```

**Example 2:**
```
Input: 
board = [
  ['a','b'],
  ['c','d']
]
words = ["abcb"]

Output: []
```

## Constraints:
- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 12`
- `board[i][j]` is a lowercase English letter.
- `1 <= words.length <= 3 * 10^4`
- `1 <= words[i].length <= 10`
- `words[i]` consists of lowercase English letters.
- All the strings of `words` are unique.

## Solution Algorithm
This problem can be efficiently solved using a combination of a Trie data structure and backtracking:

1. Build a Trie from the list of words to efficiently search for prefixes.
2. Perform a depth-first search (DFS) starting from each cell on the board.
3. During DFS, track the current path and check if it forms a valid word in the Trie.
4. Use a visited array to ensure each cell is used only once per word.
5. When a valid word is found, add it to the result set and continue searching.

### Optimizations:
- Mark found words in the Trie to avoid duplicates.
- Prune the search when a prefix is not found in the Trie.
- Remove words from the Trie after finding them to reduce unnecessary checks.

## Solution Code
```java
import java.util.*;

class Solution {
    private static final int[][] DIRECTIONS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    
    public List<String> findWords(char[][] board, String[] words) {
        // Build Trie
        TrieNode root = buildTrie(words);
        
        List<String> result = new ArrayList<>();
        int m = board.length;
        int n = board[0].length;
        boolean[][] visited = new boolean[m][n];
        
        // Search from each cell
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                dfs(board, i, j, root, visited, result);
            }
        }
        
        return result;
    }
    
    private void dfs(char[][] board, int i, int j, TrieNode node, boolean[][] visited, List<String> result) {
        // Check boundaries and if cell is already visited
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || visited[i][j]) {
            return;
        }
        
        char currentChar = board[i][j];
        TrieNode child = node.children[currentChar - 'a'];
        
        // If there's no matching child in the Trie, return
        if (child == null) {
            return;
        }
        
        // If we found a word, add it to result
        if (child.word != null) {
            result.add(child.word);
            child.word = null; // Mark as found to avoid duplicates
        }
        
        // Mark current cell as visited
        visited[i][j] = true;
        
        // Explore all four directions
        for (int[] dir : DIRECTIONS) {
            dfs(board, i + dir[0], j + dir[1], child, visited, result);
        }
        
        // Backtrack
        visited[i][j] = false;
        
        // Optimization: Remove leaf nodes that don't lead to any word
        if (isEmpty(child)) {
            node.children[currentChar - 'a'] = null;
        }
    }
    
    private boolean isEmpty(TrieNode node) {
        for (TrieNode child : node.children) {
            if (child != null) {
                return false;
            }
        }
        return true;
    }
    
    private TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        
        for (String word : words) {
            TrieNode node = root;
            
            for (char c : word.toCharArray()) {
                int index = c - 'a';
                if (node.children[index] == null) {
                    node.children[index] = new TrieNode();
                }
                node = node.children[index];
            }
            
            node.word = word; // Store the complete word at the end node
        }
        
        return root;
    }
    
    // Trie node class
    private static class TrieNode {
        TrieNode[] children = new TrieNode[26];
        String word = null; // Stores the word if this node is the end of a word
    }
}
```

```java
// Alternative implementation with more detailed comments
import java.util.*;

class Solution {
    // Define the four possible directions: right, down, left, up
    private static final int[][] DIRECTIONS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    
    public List<String> findWords(char[][] board, String[] words) {
        List<String> result = new ArrayList<>();
        
        // Edge cases
        if (board == null || board.length == 0 || words == null || words.length == 0) {
            return result;
        }
        
        // Build the Trie with all words
        TrieNode root = new TrieNode();
        for (String word : words) {
            insertWord(root, word);
        }
        
        int rows = board.length;
        int cols = board[0].length;
        boolean[][] visited = new boolean[rows][cols];
        Set<String> foundWords = new HashSet<>(); // To avoid duplicates
        
        // Start DFS from each cell
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                searchWords(board, i, j, root, visited, "", foundWords);
            }
        }
        
        // Convert set to list for the final result
        result.addAll(foundWords);
        return result;
    }
    
    private void searchWords(char[][] board, int row, int col, TrieNode node, 
                            boolean[][] visited, String currentWord, Set<String> foundWords) {
        // Check boundaries and if cell is already visited
        if (row < 0 || row >= board.length || col < 0 || col >= board[0].length || visited[row][col]) {
            return;
        }
        
        char currentChar = board[row][col];
        int index = currentChar - 'a';
        
        // If there's no matching child in the Trie, return
        if (node.children[index] == null) {
            return;
        }
        
        // Move to the next node in the Trie
        node = node.children[index];
        currentWord += currentChar;
        
        // If we found a complete word, add it to the result
        if (node.isEndOfWord) {
            foundWords.add(currentWord);
            // Note: We don't return here because one path might contain multiple words
        }
        
        // Mark current cell as visited
        visited[row][col] = true;
        
        // Explore all four directions
        for (int[] direction : DIRECTIONS) {
            int newRow = row + direction[0];
            int newCol = col + direction[1];
            searchWords(board, newRow, newCol, node, visited, currentWord, foundWords);
        }
        
        // Backtrack: mark the cell as unvisited when we're done exploring from it
        visited[row][col] = false;
    }
    
    private void insertWord(TrieNode root, String word) {
        TrieNode node = root;
        
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
            }
            node = node.children[index];
        }
        
        node.isEndOfWord = true;
    }
    
    // Trie node class
    private static class TrieNode {
        TrieNode[] children = new TrieNode[26]; // For lowercase English letters
        boolean isEndOfWord = false;
    }
}
```

```java
// Optimized solution with pruning and memory efficiency
import java.util.*;

class Solution {
    private static final int[][] DIRECTIONS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    
    public List<String> findWords(char[][] board, String[] words) {
        List<String> result = new ArrayList<>();
        
        // Edge cases
        if (board == null || board.length == 0 || words == null || words.length == 0) {
            return result;
        }
        
        // Build the Trie with all words
        TrieNode root = buildTrie(words);
        
        int rows = board.length;
        int cols = board[0].length;
        
        // Start DFS from each cell
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                dfs(board, i, j, root, result);
            }
        }
        
        return result;
    }
    
    private void dfs(char[][] board, int row, int col, TrieNode parent, List<String> result) {
        char letter = board[row][col];
        
        // If the letter is '#', it means the cell has been visited in the current path
        if (letter == '#' || parent.children[letter - 'a'] == null) {
            return;
        }
        
        TrieNode node = parent.children[letter - 'a'];
        
        // If we found a word, add it to result and mark it as found
        if (node.word != null) {
            result.add(node.word);
            node.word = null; // Avoid finding the same word again
        }
        
        // Mark the current cell as visited by changing its value
        board[row][col] = '#';
        
        // Explore all four directions
        if (row > 0) dfs(board, row - 1, col, node, result);
        if (col > 0) dfs(board, row, col - 1, node, result);
        if (row < board.length - 1) dfs(board, row + 1, col, node, result);
        if (col < board[0].length - 1) dfs(board, row, col + 1, node, result);
        
        // Restore the cell's original value
        board[row][col] = letter;
        
        // Optimization: Remove leaf nodes that don't lead to any word
        if (isEmpty(node)) {
            parent.children[letter - 'a'] = null;
        }
    }
    
    private boolean isEmpty(TrieNode node) {
        for (TrieNode child : node.children) {
            if (child != null) {
                return false;
            }
        }
        return true;
    }
    
    private TrieNode buildTrie(String[] words) {
        TrieNode root = new TrieNode();
        
        for (String word : words) {
            TrieNode node = root;
            
            for (char c : word.toCharArray()) {
                int index = c - 'a';
                if (node.children[index] == null) {
                    node.children[index] = new TrieNode();
                }
                node = node.children[index];
            }
            
            node.word = word; // Store the complete word at the end node
        }
        
        return root;
    }
    
    // Trie node class
    private static class TrieNode {
        TrieNode[] children = new TrieNode[26];
        String word = null; // Stores the word if this node is the end of a word
    }
} 