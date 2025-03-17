# LZW Compression

**Difficulty**: Medium  
**Tags**: String, Compression, Dictionary, Algorithm

## Question
Implement the Lempel-Ziv-Welch (LZW) compression algorithm, a universal lossless data compression algorithm. LZW is widely used in GIF image format and many other applications.

The LZW algorithm works by building a dictionary of previously seen strings and replacing them with codes. It starts with a dictionary containing all possible single characters and then adds new entries as it processes the input.

Your implementation should include the following functions:
1. `compress(String input)`: Compress a string using the LZW algorithm and return a list of integer codes.
2. `decompress(List<Integer> compressed)`: Decompress a list of integer codes back to the original string.

## Example Input/Output
**Input**: "ABABABA"  
**Output**:  
Compressed: [65, 66, 256, 257, 65]  
Decompressed: "ABABABA"  

**Input**: "TOBEORNOTTOBEORTOBEORNOT"  
**Output**:  
Compressed: [84, 79, 66, 69, 79, 82, 78, 79, 84, 256, 258, 260, 265, 259, 261, 263]  
Decompressed: "TOBEORNOTTOBEORTOBEORNOT"  

## Solution Algorithm
The LZW compression algorithm consists of the following steps:

1. **Compression**:
   - Initialize a dictionary with all possible single characters (ASCII values 0-255).
   - Read the first character from the input and set it as the current string.
   - For each subsequent character in the input:
     - Append the character to the current string.
     - If the resulting string is in the dictionary, update the current string and continue.
     - If the resulting string is not in the dictionary:
       - Output the code for the current string (without the new character).
       - Add the resulting string to the dictionary with a new code.
       - Set the current string to the new character.
   - Output the code for the final current string.

2. **Decompression**:
   - Initialize a dictionary with all possible single characters (ASCII values 0-255).
   - Read the first code from the compressed data and output the corresponding character.
   - Set the previous string to this character.
   - For each subsequent code in the compressed data:
     - If the code is in the dictionary, get the corresponding string.
     - If the code is not in the dictionary, construct the string using the previous string and its first character.
     - Output the string.
     - Add a new entry to the dictionary: previous string + first character of current string.
     - Set the previous string to the current string.

## Solution Code
```java
import java.util.*;

public class LZWCompression {
    // Compress a string using LZW algorithm
    public static List<Integer> compress(String input) {
        // Initialize the dictionary with all possible single characters
        Map<String, Integer> dictionary = new HashMap<>();
        for (int i = 0; i < 256; i++) {
            dictionary.put(String.valueOf((char) i), i);
        }
        
        String current = "";
        List<Integer> result = new ArrayList<>();
        int nextCode = 256; // Next available code
        
        // Process each character in the input
        for (char c : input.toCharArray()) {
            String combined = current + c;
            
            if (dictionary.containsKey(combined)) {
                // If the combined string is in the dictionary, update current
                current = combined;
            } else {
                // Output the code for the current string
                result.add(dictionary.get(current));
                
                // Add the new string to the dictionary
                dictionary.put(combined, nextCode++);
                
                // Reset current to the current character
                current = String.valueOf(c);
            }
        }
        
        // Don't forget to output the code for the final string
        if (!current.isEmpty()) {
            result.add(dictionary.get(current));
        }
        
        return result;
    }
    
    // Decompress a list of codes using LZW algorithm
    public static String decompress(List<Integer> compressed) {
        if (compressed == null || compressed.isEmpty()) {
            return "";
        }
        
        // Initialize the dictionary with all possible single characters
        Map<Integer, String> dictionary = new HashMap<>();
        for (int i = 0; i < 256; i++) {
            dictionary.put(i, String.valueOf((char) i));
        }
        
        int nextCode = 256; // Next available code
        
        // Get the first code and output its character
        int currentCode = compressed.get(0);
        String current = dictionary.get(currentCode);
        StringBuilder result = new StringBuilder(current);
        
        // Process the rest of the codes
        for (int i = 1; i < compressed.size(); i++) {
            int code = compressed.get(i);
            String entry;
            
            if (dictionary.containsKey(code)) {
                // If the code is in the dictionary, get its string
                entry = dictionary.get(code);
            } else if (code == nextCode) {
                // Special case: the code is not in the dictionary yet
                // This happens when the next code would be the one we're looking for
                entry = current + current.charAt(0);
            } else {
                throw new IllegalArgumentException("Invalid compressed data");
            }
            
            // Append the entry to the result
            result.append(entry);
            
            // Add a new entry to the dictionary
            dictionary.put(nextCode++, current + entry.charAt(0));
            
            // Update current
            current = entry;
        }
        
        return result.toString();
    }
    
    public static void main(String[] args) {
        String input1 = "ABABABA";
        List<Integer> compressed1 = compress(input1);
        System.out.println("Original: " + input1);
        System.out.println("Compressed: " + compressed1);
        System.out.println("Decompressed: " + decompress(compressed1));
        System.out.println();
        
        String input2 = "TOBEORNOTTOBEORTOBEORNOT";
        List<Integer> compressed2 = compress(input2);
        System.out.println("Original: " + input2);
        System.out.println("Compressed: " + compressed2);
        System.out.println("Decompressed: " + decompress(compressed2));
    }
}
```

```java
// Alternative implementation with more detailed comments and dictionary size limit
import java.util.*;

public class LZWCompressionWithLimit {
    private static final int MAX_DICT_SIZE = 4096; // 12-bit codes (common in GIF)
    
    // Compress a string using LZW algorithm with dictionary size limit
    public static List<Integer> compress(String input) {
        // Initialize the dictionary with all possible single characters
        Map<String, Integer> dictionary = new HashMap<>();
        for (int i = 0; i < 256; i++) {
            dictionary.put(String.valueOf((char) i), i);
        }
        
        String current = "";
        List<Integer> result = new ArrayList<>();
        int nextCode = 256; // Next available code
        
        // Process each character in the input
        for (char c : input.toCharArray()) {
            String combined = current + c;
            
            if (dictionary.containsKey(combined)) {
                // If the combined string is in the dictionary, update current
                current = combined;
            } else {
                // Output the code for the current string
                result.add(dictionary.get(current));
                
                // Add the new string to the dictionary if there's room
                if (nextCode < MAX_DICT_SIZE) {
                    dictionary.put(combined, nextCode++);
                }
                
                // Reset current to the current character
                current = String.valueOf(c);
            }
        }
        
        // Don't forget to output the code for the final string
        if (!current.isEmpty()) {
            result.add(dictionary.get(current));
        }
        
        return result;
    }
    
    // Decompress a list of codes using LZW algorithm with dictionary size limit
    public static String decompress(List<Integer> compressed) {
        if (compressed == null || compressed.isEmpty()) {
            return "";
        }
        
        // Initialize the dictionary with all possible single characters
        Map<Integer, String> dictionary = new HashMap<>();
        for (int i = 0; i < 256; i++) {
            dictionary.put(i, String.valueOf((char) i));
        }
        
        int nextCode = 256; // Next available code
        
        // Get the first code and output its character
        int currentCode = compressed.get(0);
        String current = dictionary.get(currentCode);
        StringBuilder result = new StringBuilder(current);
        
        // Process the rest of the codes
        for (int i = 1; i < compressed.size(); i++) {
            int code = compressed.get(i);
            String entry;
            
            if (dictionary.containsKey(code)) {
                // If the code is in the dictionary, get its string
                entry = dictionary.get(code);
            } else if (code == nextCode) {
                // Special case: the code is not in the dictionary yet
                // This happens when the next code would be the one we're looking for
                entry = current + current.charAt(0);
            } else {
                throw new IllegalArgumentException("Invalid compressed data");
            }
            
            // Append the entry to the result
            result.append(entry);
            
            // Add a new entry to the dictionary if there's room
            if (nextCode < MAX_DICT_SIZE) {
                dictionary.put(nextCode++, current + entry.charAt(0));
            }
            
            // Update current
            current = entry;
        }
        
        return result.toString();
    }
    
    // Calculate compression ratio
    public static double compressionRatio(String original, List<Integer> compressed) {
        // Original size in bits (assuming 8 bits per character)
        int originalSize = original.length() * 8;
        
        // Compressed size in bits (assuming 12 bits per code for LZW)
        int compressedSize = compressed.size() * 12;
        
        return (double) originalSize / compressedSize;
    }
    
    public static void main(String[] args) {
        String input1 = "ABABABA";
        List<Integer> compressed1 = compress(input1);
        System.out.println("Original: " + input1);
        System.out.println("Compressed: " + compressed1);
        System.out.println("Decompressed: " + decompress(compressed1));
        System.out.println("Compression ratio: " + compressionRatio(input1, compressed1));
        System.out.println();
        
        String input2 = "TOBEORNOTTOBEORTOBEORNOT";
        List<Integer> compressed2 = compress(input2);
        System.out.println("Original: " + input2);
        System.out.println("Compressed: " + compressed2);
        System.out.println("Decompressed: " + decompress(compressed2));
        System.out.println("Compression ratio: " + compressionRatio(input2, compressed2));
        System.out.println();
        
        // Test with a repeating pattern (should compress well)
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 100; i++) {
            sb.append("ABCDEFG");
        }
        String input3 = sb.toString();
        List<Integer> compressed3 = compress(input3);
        System.out.println("Original length: " + input3.length());
        System.out.println("Compressed length: " + compressed3.size());
        String decompressed3 = decompress(compressed3);
        System.out.println("Decompressed length: " + decompressed3.length());
        System.out.println("Compression ratio: " + compressionRatio(input3, compressed3));
        System.out.println("Correctly decompressed: " + input3.equals(decompressed3));
    }
}
```

```java
// Implementation with dictionary reset when it gets full
import java.util.*;

public class LZWCompressionWithReset {
    private static final int MAX_DICT_SIZE = 4096; // 12-bit codes
    private static final int CLEAR_CODE = 256; // Special code to reset the dictionary
    
    // Compress a string using LZW algorithm with dictionary reset
    public static List<Integer> compress(String input) {
        // Initialize the dictionary with all possible single characters
        Map<String, Integer> dictionary = new HashMap<>();
        for (int i = 0; i < 256; i++) {
            dictionary.put(String.valueOf((char) i), i);
        }
        
        String current = "";
        List<Integer> result = new ArrayList<>();
        int nextCode = 257; // Next available code (257 because 256 is the CLEAR_CODE)
        
        // Add the clear code at the beginning
        result.add(CLEAR_CODE);
        
        // Process each character in the input
        for (char c : input.toCharArray()) {
            String combined = current + c;
            
            if (dictionary.containsKey(combined)) {
                // If the combined string is in the dictionary, update current
                current = combined;
            } else {
                // Output the code for the current string
                result.add(dictionary.get(current));
                
                // Add the new string to the dictionary
                dictionary.put(combined, nextCode++);
                
                // Reset current to the current character
                current = String.valueOf(c);
                
                // If the dictionary is full, reset it
                if (nextCode >= MAX_DICT_SIZE) {
                    result.add(CLEAR_CODE);
                    dictionary.clear();
                    for (int i = 0; i < 256; i++) {
                        dictionary.put(String.valueOf((char) i), i);
                    }
                    nextCode = 257;
                }
            }
        }
        
        // Don't forget to output the code for the final string
        if (!current.isEmpty()) {
            result.add(dictionary.get(current));
        }
        
        return result;
    }
    
    // Decompress a list of codes using LZW algorithm with dictionary reset
    public static String decompress(List<Integer> compressed) {
        if (compressed == null || compressed.isEmpty()) {
            return "";
        }
        
        // Initialize the dictionary with all possible single characters
        Map<Integer, String> dictionary = new HashMap<>();
        for (int i = 0; i < 256; i++) {
            dictionary.put(i, String.valueOf((char) i));
        }
        
        StringBuilder result = new StringBuilder();
        int nextCode = 257; // Next available code (257 because 256 is the CLEAR_CODE)
        String current = null;
        
        // Process each code in the compressed data
        for (int i = 0; i < compressed.size(); i++) {
            int code = compressed.get(i);
            
            // Check if it's a clear code
            if (code == CLEAR_CODE) {
                // Reset the dictionary
                dictionary.clear();
                for (int j = 0; j < 256; j++) {
                    dictionary.put(j, String.valueOf((char) j));
                }
                nextCode = 257;
                
                // Skip to the next code
                if (i + 1 < compressed.size()) {
                    code = compressed.get(++i);
                    current = dictionary.get(code);
                    result.append(current);
                }
                continue;
            }
            
            String entry;
            
            if (dictionary.containsKey(code)) {
                // If the code is in the dictionary, get its string
                entry = dictionary.get(code);
            } else if (code == nextCode && current != null) {
                // Special case: the code is not in the dictionary yet
                entry = current + current.charAt(0);
            } else {
                throw new IllegalArgumentException("Invalid compressed data at index " + i);
            }
            
            // Append the entry to the result
            result.append(entry);
            
            // Add a new entry to the dictionary if we have a previous string
            if (current != null) {
                dictionary.put(nextCode++, current + entry.charAt(0));
            }
            
            // Update current
            current = entry;
            
            // If the dictionary is full, it will be reset on the next CLEAR_CODE
        }
        
        return result.toString();
    }
    
    public static void main(String[] args) {
        String input = "TOBEORNOTTOBEORTOBEORNOT";
        List<Integer> compressed = compress(input);
        System.out.println("Original: " + input);
        System.out.println("Compressed: " + compressed);
        String decompressed = decompress(compressed);
        System.out.println("Decompressed: " + decompressed);
        System.out.println("Correctly decompressed: " + input.equals(decompressed));
        
        // Test with a long repeating pattern to force dictionary reset
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 1000; i++) {
            sb.append((char) ('A' + (i % 26)));
        }
        String longInput = sb.toString();
        List<Integer> longCompressed = compress(longInput);
        String longDecompressed = decompress(longCompressed);
        System.out.println("\nLong input length: " + longInput.length());
        System.out.println("Long compressed length: " + longCompressed.size());
        System.out.println("Long decompressed length: " + longDecompressed.length());
        System.out.println("Correctly decompressed: " + longInput.equals(longDecompressed));
    }
} 