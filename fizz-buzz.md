# Fizz Buzz

**Difficulty**: Easy  
**Tags**: Math, String, Simulation

## Question
Given an integer `n`, return a string array `answer` (1-indexed) where:

- `answer[i] == "FizzBuzz"` if `i` is divisible by 3 and 5.
- `answer[i] == "Fizz"` if `i` is divisible by 3.
- `answer[i] == "Buzz"` if `i` is divisible by 5.
- `answer[i] == i` (as a string) if none of the above conditions are true.

## Example Input/Output
**Input**: n = 3  
**Output**: ["1","2","Fizz"]

**Input**: n = 5  
**Output**: ["1","2","Fizz","4","Buzz"]

**Input**: n = 15  
**Output**: ["1","2","Fizz","4","Buzz","Fizz","7","8","Fizz","Buzz","11","Fizz","13","14","FizzBuzz"]

## Solution Algorithm
This is a classic programming problem that can be solved with a straightforward approach:

1. Create an empty array to store the results.
2. Iterate from 1 to n.
3. For each number:
   - If it's divisible by both 3 and 5, add "FizzBuzz" to the array.
   - If it's divisible by 3 (but not 5), add "Fizz" to the array.
   - If it's divisible by 5 (but not 3), add "Buzz" to the array.
   - Otherwise, add the number itself (as a string) to the array.
4. Return the array.

## Solution Code
```java
public List<String> fizzBuzz(int n) {
    List<String> answer = new ArrayList<>();
    
    for (int i = 1; i <= n; i++) {
        if (i % 3 == 0 && i % 5 == 0) {
            answer.add("FizzBuzz");
        } else if (i % 3 == 0) {
            answer.add("Fizz");
        } else if (i % 5 == 0) {
            answer.add("Buzz");
        } else {
            answer.add(String.valueOf(i));
        }
    }
    
    return answer;
}
```

```java
// Alternative implementation with less repetitive divisibility checks
public List<String> fizzBuzz(int n) {
    List<String> answer = new ArrayList<>();
    
    for (int i = 1; i <= n; i++) {
        boolean divisibleBy3 = (i % 3 == 0);
        boolean divisibleBy5 = (i % 5 == 0);
        
        String currentStr = "";
        
        if (divisibleBy3) {
            currentStr += "Fizz";
        }
        
        if (divisibleBy5) {
            currentStr += "Buzz";
        }
        
        if (currentStr.isEmpty()) {
            currentStr += String.valueOf(i);
        }
        
        answer.add(currentStr);
    }
    
    return answer;
}
``` 