# RSA Encryption

**Difficulty**: Hard  
**Tags**: Math, Encryption, Decryption, Number Theory

## Question
Implement the RSA (Rivest–Shamir–Adleman) encryption algorithm, one of the first public-key cryptosystems widely used for secure data transmission. The security of RSA relies on the practical difficulty of factoring the product of two large prime numbers.

Your implementation should include the following functions:
1. `generateKeyPair(int keySize)`: Generate a public and private key pair.
2. `encrypt(BigInteger message, PublicKey publicKey)`: Encrypt a message using the public key.
3. `decrypt(BigInteger ciphertext, PrivateKey privateKey)`: Decrypt a ciphertext using the private key.

For simplicity, you can assume that the message is a BigInteger that is smaller than the modulus n.

## Example Input/Output
**Input**:  
- Key size: 1024 bits
- Message: 123456789

**Output**:  
- Public key: (n, e) where n is the modulus and e is the public exponent
- Private key: (n, d) where d is the private exponent
- Encrypted message: [a large number]
- Decrypted message: 123456789

## Solution Algorithm
The RSA algorithm consists of the following steps:

1. **Key Generation**:
   - Choose two distinct prime numbers p and q.
   - Compute n = p * q. This is the modulus for both the public and private keys.
   - Compute the totient: φ(n) = (p - 1) * (q - 1).
   - Choose an integer e such that 1 < e < φ(n) and gcd(e, φ(n)) = 1; i.e., e and φ(n) are coprime. This is the public exponent.
   - Determine d as d ≡ e^(-1) (mod φ(n)); i.e., d is the modular multiplicative inverse of e modulo φ(n). This is the private exponent.
   - The public key is (n, e) and the private key is (n, d).

2. **Encryption**:
   - Convert the message to a number m.
   - Compute the ciphertext c = m^e mod n.

3. **Decryption**:
   - Compute the original message m = c^d mod n.

## Solution Code
```java
import java.math.BigInteger;
import java.security.SecureRandom;

public class RSA {
    private BigInteger n; // modulus
    private BigInteger e; // public exponent
    private BigInteger d; // private exponent
    private BigInteger p; // prime factor 1
    private BigInteger q; // prime factor 2
    private BigInteger phi; // totient
    
    // Generate a key pair with the specified bit length
    public void generateKeyPair(int keySize) {
        SecureRandom random = new SecureRandom();
        
        // Step 1: Generate two large prime numbers
        p = BigInteger.probablePrime(keySize / 2, random);
        q = BigInteger.probablePrime(keySize / 2, random);
        
        // Step 2: Compute n = p * q
        n = p.multiply(q);
        
        // Step 3: Compute the totient φ(n) = (p - 1) * (q - 1)
        phi = p.subtract(BigInteger.ONE).multiply(q.subtract(BigInteger.ONE));
        
        // Step 4: Choose e such that 1 < e < φ(n) and gcd(e, φ(n)) = 1
        e = BigInteger.valueOf(65537); // Common value for e
        
        // Ensure e and phi are coprime
        while (phi.gcd(e).compareTo(BigInteger.ONE) > 0 && e.compareTo(phi) < 0) {
            e = e.add(BigInteger.TWO);
        }
        
        // Step 5: Compute d, the modular multiplicative inverse of e modulo φ(n)
        d = e.modInverse(phi);
        
        System.out.println("Public Key (n, e): (" + n + ", " + e + ")");
        System.out.println("Private Key (n, d): (" + n + ", " + d + ")");
    }
    
    // Encrypt a message using the public key
    public BigInteger encrypt(BigInteger message) {
        if (message.compareTo(n) >= 0) {
            throw new IllegalArgumentException("Message must be less than n");
        }
        
        // c = m^e mod n
        return message.modPow(e, n);
    }
    
    // Decrypt a ciphertext using the private key
    public BigInteger decrypt(BigInteger ciphertext) {
        // m = c^d mod n
        return ciphertext.modPow(d, n);
    }
    
    public static void main(String[] args) {
        RSA rsa = new RSA();
        
        // Generate a 1024-bit key pair
        rsa.generateKeyPair(1024);
        
        // Example message
        BigInteger message = new BigInteger("123456789");
        System.out.println("Original Message: " + message);
        
        // Encrypt the message
        BigInteger ciphertext = rsa.encrypt(message);
        System.out.println("Encrypted Message: " + ciphertext);
        
        // Decrypt the ciphertext
        BigInteger decrypted = rsa.decrypt(ciphertext);
        System.out.println("Decrypted Message: " + decrypted);
    }
}
```

```java
// More comprehensive implementation with additional features
import java.math.BigInteger;
import java.security.SecureRandom;
import java.util.Base64;
import java.nio.charset.StandardCharsets;

public class RSAAdvanced {
    // Key components
    private BigInteger n; // modulus
    private BigInteger e; // public exponent
    private BigInteger d; // private exponent
    private BigInteger p; // prime factor 1
    private BigInteger q; // prime factor 2
    private BigInteger phi; // totient
    
    // Public key class
    public static class PublicKey {
        private final BigInteger n;
        private final BigInteger e;
        
        public PublicKey(BigInteger n, BigInteger e) {
            this.n = n;
            this.e = e;
        }
        
        public BigInteger getN() { return n; }
        public BigInteger getE() { return e; }
        
        @Override
        public String toString() {
            return "PublicKey(n=" + n + ", e=" + e + ")";
        }
    }
    
    // Private key class
    public static class PrivateKey {
        private final BigInteger n;
        private final BigInteger d;
        
        public PrivateKey(BigInteger n, BigInteger d) {
            this.n = n;
            this.d = d;
        }
        
        public BigInteger getN() { return n; }
        public BigInteger getD() { return d; }
        
        @Override
        public String toString() {
            return "PrivateKey(n=" + n + ", d=" + d + ")";
        }
    }
    
    // Generate a key pair with the specified bit length
    public static KeyPair generateKeyPair(int keySize) {
        SecureRandom random = new SecureRandom();
        
        // Step 1: Generate two large prime numbers
        BigInteger p = BigInteger.probablePrime(keySize / 2, random);
        BigInteger q = BigInteger.probablePrime(keySize / 2, random);
        
        // Step 2: Compute n = p * q
        BigInteger n = p.multiply(q);
        
        // Step 3: Compute the totient φ(n) = (p - 1) * (q - 1)
        BigInteger phi = p.subtract(BigInteger.ONE).multiply(q.subtract(BigInteger.ONE));
        
        // Step 4: Choose e such that 1 < e < φ(n) and gcd(e, φ(n)) = 1
        BigInteger e = BigInteger.valueOf(65537); // Common value for e
        
        // Ensure e and phi are coprime
        while (phi.gcd(e).compareTo(BigInteger.ONE) > 0 && e.compareTo(phi) < 0) {
            e = e.add(BigInteger.TWO);
        }
        
        // Step 5: Compute d, the modular multiplicative inverse of e modulo φ(n)
        BigInteger d = e.modInverse(phi);
        
        PublicKey publicKey = new PublicKey(n, e);
        PrivateKey privateKey = new PrivateKey(n, d);
        
        return new KeyPair(publicKey, privateKey);
    }
    
    // Key pair class
    public static class KeyPair {
        private final PublicKey publicKey;
        private final PrivateKey privateKey;
        
        public KeyPair(PublicKey publicKey, PrivateKey privateKey) {
            this.publicKey = publicKey;
            this.privateKey = privateKey;
        }
        
        public PublicKey getPublicKey() { return publicKey; }
        public PrivateKey getPrivateKey() { return privateKey; }
    }
    
    // Encrypt a message using the public key
    public static BigInteger encrypt(BigInteger message, PublicKey publicKey) {
        if (message.compareTo(publicKey.getN()) >= 0) {
            throw new IllegalArgumentException("Message must be less than n");
        }
        
        // c = m^e mod n
        return message.modPow(publicKey.getE(), publicKey.getN());
    }
    
    // Decrypt a ciphertext using the private key
    public static BigInteger decrypt(BigInteger ciphertext, PrivateKey privateKey) {
        // m = c^d mod n
        return ciphertext.modPow(privateKey.getD(), privateKey.getN());
    }
    
    // Encrypt a string message
    public static String encryptString(String message, PublicKey publicKey) {
        // Convert string to bytes
        byte[] messageBytes = message.getBytes(StandardCharsets.UTF_8);
        
        // Convert bytes to BigInteger
        BigInteger messageBigInt = new BigInteger(1, messageBytes);
        
        // Encrypt
        BigInteger cipherBigInt = encrypt(messageBigInt, publicKey);
        
        // Convert to Base64 for easier handling
        return Base64.getEncoder().encodeToString(cipherBigInt.toByteArray());
    }
    
    // Decrypt a string ciphertext
    public static String decryptString(String ciphertext, PrivateKey privateKey) {
        // Decode from Base64
        byte[] cipherBytes = Base64.getDecoder().decode(ciphertext);
        
        // Convert to BigInteger
        BigInteger cipherBigInt = new BigInteger(cipherBytes);
        
        // Decrypt
        BigInteger messageBigInt = decrypt(cipherBigInt, privateKey);
        
        // Convert back to string
        byte[] messageBytes = messageBigInt.toByteArray();
        if (messageBytes[0] == 0) {
            // Remove padding byte if present
            byte[] tmp = new byte[messageBytes.length - 1];
            System.arraycopy(messageBytes, 1, tmp, 0, tmp.length);
            messageBytes = tmp;
        }
        
        return new String(messageBytes, StandardCharsets.UTF_8);
    }
    
    public static void main(String[] args) {
        // Generate a 1024-bit key pair
        KeyPair keyPair = generateKeyPair(1024);
        PublicKey publicKey = keyPair.getPublicKey();
        PrivateKey privateKey = keyPair.getPrivateKey();
        
        System.out.println("Public Key: " + publicKey);
        System.out.println("Private Key: " + privateKey);
        
        // Example numeric message
        BigInteger message = new BigInteger("123456789");
        System.out.println("\nOriginal Numeric Message: " + message);
        
        // Encrypt the numeric message
        BigInteger ciphertext = encrypt(message, publicKey);
        System.out.println("Encrypted Numeric Message: " + ciphertext);
        
        // Decrypt the numeric ciphertext
        BigInteger decrypted = decrypt(ciphertext, privateKey);
        System.out.println("Decrypted Numeric Message: " + decrypted);
        
        // Example string message
        String stringMessage = "Hello, RSA!";
        System.out.println("\nOriginal String Message: " + stringMessage);
        
        // Encrypt the string message
        String encryptedString = encryptString(stringMessage, publicKey);
        System.out.println("Encrypted String Message: " + encryptedString);
        
        // Decrypt the string ciphertext
        String decryptedString = decryptString(encryptedString, privateKey);
        System.out.println("Decrypted String Message: " + decryptedString);
    }
}
```

```java
// Implementation with Chinese Remainder Theorem for faster decryption
import java.math.BigInteger;
import java.security.SecureRandom;

public class RSAWithCRT {
    // Key components
    private BigInteger n; // modulus
    private BigInteger e; // public exponent
    private BigInteger d; // private exponent
    private BigInteger p; // prime factor 1
    private BigInteger q; // prime factor 2
    private BigInteger dP; // d mod (p-1)
    private BigInteger dQ; // d mod (q-1)
    private BigInteger qInv; // q^(-1) mod p
    
    // Generate a key pair with the specified bit length
    public void generateKeyPair(int keySize) {
        SecureRandom random = new SecureRandom();
        
        // Step 1: Generate two large prime numbers
        p = BigInteger.probablePrime(keySize / 2, random);
        q = BigInteger.probablePrime(keySize / 2, random);
        
        // Step 2: Compute n = p * q
        n = p.multiply(q);
        
        // Step 3: Compute the totient φ(n) = (p - 1) * (q - 1)
        BigInteger phi = p.subtract(BigInteger.ONE).multiply(q.subtract(BigInteger.ONE));
        
        // Step 4: Choose e such that 1 < e < φ(n) and gcd(e, φ(n)) = 1
        e = BigInteger.valueOf(65537); // Common value for e
        
        // Ensure e and phi are coprime
        while (phi.gcd(e).compareTo(BigInteger.ONE) > 0 && e.compareTo(phi) < 0) {
            e = e.add(BigInteger.TWO);
        }
        
        // Step 5: Compute d, the modular multiplicative inverse of e modulo φ(n)
        d = e.modInverse(phi);
        
        // Compute CRT parameters
        dP = d.mod(p.subtract(BigInteger.ONE));
        dQ = d.mod(q.subtract(BigInteger.ONE));
        qInv = q.modInverse(p);
        
        System.out.println("Public Key (n, e): (" + n + ", " + e + ")");
        System.out.println("Private Key (n, d): (" + n + ", " + d + ")");
    }
    
    // Encrypt a message using the public key
    public BigInteger encrypt(BigInteger message) {
        if (message.compareTo(n) >= 0) {
            throw new IllegalArgumentException("Message must be less than n");
        }
        
        // c = m^e mod n
        return message.modPow(e, n);
    }
    
    // Decrypt a ciphertext using the private key (standard method)
    public BigInteger decryptStandard(BigInteger ciphertext) {
        // m = c^d mod n
        return ciphertext.modPow(d, n);
    }
    
    // Decrypt a ciphertext using the Chinese Remainder Theorem (faster)
    public BigInteger decryptCRT(BigInteger ciphertext) {
        // m1 = c^dP mod p
        BigInteger m1 = ciphertext.modPow(dP, p);
        
        // m2 = c^dQ mod q
        BigInteger m2 = ciphertext.modPow(dQ, q);
        
        // h = qInv * (m1 - m2) mod p
        BigInteger h = qInv.multiply(m1.subtract(m2).mod(p)).mod(p);
        
        // m = m2 + h * q
        return m2.add(h.multiply(q));
    }
    
    public static void main(String[] args) {
        RSAWithCRT rsa = new RSAWithCRT();
        
        // Generate a 1024-bit key pair
        rsa.generateKeyPair(1024);
        
        // Example message
        BigInteger message = new BigInteger("123456789");
        System.out.println("Original Message: " + message);
        
        // Encrypt the message
        BigInteger ciphertext = rsa.encrypt(message);
        System.out.println("Encrypted Message: " + ciphertext);
        
        // Decrypt using standard method
        long startStandard = System.nanoTime();
        BigInteger decryptedStandard = rsa.decryptStandard(ciphertext);
        long endStandard = System.nanoTime();
        System.out.println("Decrypted Message (Standard): " + decryptedStandard);
        System.out.println("Standard Decryption Time: " + (endStandard - startStandard) + " ns");
        
        // Decrypt using CRT method
        long startCRT = System.nanoTime();
        BigInteger decryptedCRT = rsa.decryptCRT(ciphertext);
        long endCRT = System.nanoTime();
        System.out.println("Decrypted Message (CRT): " + decryptedCRT);
        System.out.println("CRT Decryption Time: " + (endCRT - startCRT) + " ns");
        
        // Verify both methods give the same result
        System.out.println("Results match: " + decryptedStandard.equals(decryptedCRT));
    }
} 