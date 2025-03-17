# Roman to Integer

**Difficulty**: Easy  
**Tags**: String, Math, Hash Table

## Question
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

```
Symbol       Value
I            1
V            5
X            10
L            50
C            100
D            500
M            1000
```

For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX.

There are six instances where subtraction is used:
- I can be placed before V (5) and X (10) to make 4 and 9. 
- X can be placed before L (50) and C (100) to make 40 and 90. 
- C can be placed before D (500) and M (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.

## Example Input/Output
**Input**: s = "III"  
**Output**: 3

**Input**: s = "IV"  
**Output**: 4

**Input**: s = "IX"  
**Output**: 9

**Input**: s = "LVIII"  
**Output**: 58  
**Explanation**: L = 50, V= 5, III = 3.

**Input**: s = "MCMXCIV"  
**Output**: 1994  
**Explanation**: M = 1000, CM = 900, XC = 90 and IV = 4.

## Solution Algorithm
1. Create a hash map to store the values of Roman numerals
2. Initialize a variable to store the result
3. Iterate through the string from left to right:
   - If the current value is greater than or equal to the next value, add it to the result
   - If the current value is less than the next value, subtract it from the result
4. Return the result

## Solution Code
```java
public int romanToInt(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    
    // Create a map to store Roman numeral values
    Map<Character, Integer> romanValues = new HashMap<>();
    romanValues.put('I', 1);
    romanValues.put('V', 5);
    romanValues.put('X', 10);
    romanValues.put('L', 50);
    romanValues.put('C', 100);
    romanValues.put('D', 500);
    romanValues.put('M', 1000);
    
    int result = 0;
    int prevValue = 0;
    
    // Traverse the string from right to left
    for (int i = s.length() - 1; i >= 0; i--) {
        int currentValue = romanValues.get(s.charAt(i));
        
        // If current value is greater than or equal to previous value, add it
        // Otherwise subtract it (for cases like IV, IX, etc.)
        if (currentValue >= prevValue) {
            result += currentValue;
        } else {
            result -= currentValue;
        }
        
        prevValue = currentValue;
    }
    
    return result;
}
```

// Alternative implementation using left-to-right traversal
```java
public int romanToInt(String s) {
    Map<Character, Integer> romanValues = new HashMap<>();
    romanValues.put('I', 1);
    romanValues.put('V', 5);
    romanValues.put('X', 10);
    romanValues.put('L', 50);
    romanValues.put('C', 100);
    romanValues.put('D', 500);
    romanValues.put('M', 1000);
    
    int result = 0;
    
    for (int i = 0; i < s.length(); i++) {
        // If this is the last character or current value >= next value
        if (i == s.length() - 1 || romanValues.get(s.charAt(i)) >= romanValues.get(s.charAt(i + 1))) {
            result += romanValues.get(s.charAt(i));
        } else {
            // Current value < next value (like IV, IX, etc.)
            result -= romanValues.get(s.charAt(i));
        }
    }
    
    return result;
}
``` 