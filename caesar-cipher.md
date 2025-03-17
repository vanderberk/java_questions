# Caesar Cipher

**Difficulty**: Easy  
**Tags**: String, Encryption, Decryption

## Question
Implement the Caesar Cipher, one of the simplest and most widely known encryption techniques. The transformation is named after Julius Caesar, who used it in his private correspondence.

The Caesar Cipher is a substitution cipher where each letter in the plaintext is shifted a certain number of places down or up the alphabet. For example, with a left shift of 3, 'D' would be replaced by 'A', 'E' would become 'B', and so on.

Write two functions:
1. `encrypt(String plaintext, int shift)`: Encrypts a plaintext string using the Caesar Cipher with the given shift.
2. `decrypt(String ciphertext, int shift)`: Decrypts a ciphertext string that was encrypted using the Caesar Cipher with the given shift.

Assume that the input string contains only uppercase letters (A-Z).

## Example Input/Output
**Encryption**:  
**Input**: plaintext = "HELLO", shift = 3  
**Output**: "KHOOR"

**Decryption**:  
**Input**: ciphertext = "KHOOR", shift = 3  
**Output**: "HELLO"

## Solution Algorithm
The algorithm for the Caesar Cipher is straightforward:

**Encryption**:
1. For each character in the plaintext:
   - Convert the character to its ASCII value.
   - Add the shift value to the ASCII value.
   - If the result exceeds the ASCII value of 'Z', wrap around to the beginning of the alphabet.
   - Convert the new ASCII value back to a character and append it to the result.
2. Return the encrypted string.

**Decryption**:
1. For each character in the ciphertext:
   - Convert the character to its ASCII value.
   - Subtract the shift value from the ASCII value.
   - If the result is less than the ASCII value of 'A', wrap around to the end of the alphabet.
   - Convert the new ASCII value back to a character and append it to the result.
2. Return the decrypted string.

## Solution Code
```java
public class CaesarCipher {
    // Encrypt a plaintext string using the Caesar Cipher
    public static String encrypt(String plaintext, int shift) {
        StringBuilder encrypted = new StringBuilder();
        
        for (char c : plaintext.toCharArray()) {
            if (Character.isUpperCase(c)) {
                // Apply the shift and handle wrapping
                char shifted = (char) ((c - 'A' + shift) % 26 + 'A');
                encrypted.append(shifted);
            } else {
                // Keep non-alphabetic characters as is
                encrypted.append(c);
            }
        }
        
        return encrypted.toString();
    }
    
    // Decrypt a ciphertext string using the Caesar Cipher
    public static String decrypt(String ciphertext, int shift) {
        // Decryption is just encryption with the negative shift
        return encrypt(ciphertext, 26 - (shift % 26));
    }
    
    public static void main(String[] args) {
        String plaintext = "HELLO";
        int shift = 3;
        
        String encrypted = encrypt(plaintext, shift);
        System.out.println("Encrypted: " + encrypted);
        
        String decrypted = decrypt(encrypted, shift);
        System.out.println("Decrypted: " + decrypted);
    }
}
```

```java
// Alternative implementation with more detailed comments and handling for all ASCII characters
public class CaesarCipher {
    // Encrypt a plaintext string using the Caesar Cipher
    public static String encrypt(String plaintext, int shift) {
        // Normalize the shift value to be between 0 and 25
        shift = shift % 26;
        if (shift < 0) {
            shift += 26;
        }
        
        StringBuilder encrypted = new StringBuilder();
        
        for (char c : plaintext.toCharArray()) {
            if (Character.isLetter(c)) {
                // Determine if the character is uppercase or lowercase
                char base = Character.isUpperCase(c) ? 'A' : 'a';
                
                // Apply the shift and handle wrapping
                // Subtract 'A' or 'a' to get a value between 0 and 25
                // Add the shift, take modulo 26 to handle wrapping
                // Add 'A' or 'a' back to get the ASCII value
                char shifted = (char) ((c - base + shift) % 26 + base);
                encrypted.append(shifted);
            } else {
                // Keep non-alphabetic characters as is
                encrypted.append(c);
            }
        }
        
        return encrypted.toString();
    }
    
    // Decrypt a ciphertext string using the Caesar Cipher
    public static String decrypt(String ciphertext, int shift) {
        // Normalize the shift value to be between 0 and 25
        shift = shift % 26;
        if (shift < 0) {
            shift += 26;
        }
        
        // Decryption is just encryption with the negative shift
        return encrypt(ciphertext, 26 - shift);
    }
    
    // Brute force all possible shifts (useful when the shift is unknown)
    public static void bruteForce(String ciphertext) {
        System.out.println("Brute force decryption attempts:");
        for (int shift = 0; shift < 26; shift++) {
            String attempt = decrypt(ciphertext, shift);
            System.out.println("Shift " + shift + ": " + attempt);
        }
    }
    
    public static void main(String[] args) {
        String plaintext = "HELLO";
        int shift = 3;
        
        String encrypted = encrypt(plaintext, shift);
        System.out.println("Encrypted: " + encrypted);
        
        String decrypted = decrypt(encrypted, shift);
        System.out.println("Decrypted: " + decrypted);
        
        // Demonstrate brute force decryption
        bruteForce(encrypted);
    }
}
```

```java
// Implementation with frequency analysis for automatic decryption
import java.util.*;

public class CaesarCipherWithFrequencyAnalysis {
    // English letter frequency (from most common to least common)
    private static final char[] ENGLISH_FREQ = 
        {'E', 'T', 'A', 'O', 'I', 'N', 'S', 'H', 'R', 'D', 'L', 'C', 'U', 'M', 'W', 'F', 'G', 'Y', 'P', 'B', 'V', 'K', 'J', 'X', 'Q', 'Z'};
    
    // Encrypt a plaintext string using the Caesar Cipher
    public static String encrypt(String plaintext, int shift) {
        StringBuilder encrypted = new StringBuilder();
        
        for (char c : plaintext.toCharArray()) {
            if (Character.isUpperCase(c)) {
                char shifted = (char) ((c - 'A' + shift) % 26 + 'A');
                encrypted.append(shifted);
            } else if (Character.isLowerCase(c)) {
                char shifted = (char) ((c - 'a' + shift) % 26 + 'a');
                encrypted.append(shifted);
            } else {
                encrypted.append(c);
            }
        }
        
        return encrypted.toString();
    }
    
    // Decrypt a ciphertext string using the Caesar Cipher
    public static String decrypt(String ciphertext, int shift) {
        return encrypt(ciphertext, 26 - (shift % 26));
    }
    
    // Automatically decrypt using frequency analysis
    public static String autoDecrypt(String ciphertext) {
        // Count the frequency of each letter in the ciphertext
        int[] frequency = new int[26];
        int totalLetters = 0;
        
        for (char c : ciphertext.toCharArray()) {
            if (Character.isLetter(c)) {
                char upper = Character.toUpperCase(c);
                frequency[upper - 'A']++;
                totalLetters++;
            }
        }
        
        // Find the most frequent letter in the ciphertext
        int maxIndex = 0;
        for (int i = 1; i < 26; i++) {
            if (frequency[i] > frequency[maxIndex]) {
                maxIndex = i;
            }
        }
        
        // Assume the most frequent letter corresponds to 'E' (the most common letter in English)
        // Calculate the shift that would have been used
        int likelyShift = (maxIndex - ('E' - 'A') + 26) % 26;
        
        // Decrypt using the likely shift
        return decrypt(ciphertext, likelyShift);
    }
    
    public static void main(String[] args) {
        String plaintext = "THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG";
        int shift = 7;
        
        String encrypted = encrypt(plaintext, shift);
        System.out.println("Encrypted: " + encrypted);
        
        String decrypted = decrypt(encrypted, shift);
        System.out.println("Decrypted with known shift: " + decrypted);
        
        String autoDecrypted = autoDecrypt(encrypted);
        System.out.println("Auto-decrypted: " + autoDecrypted);
    }
} 