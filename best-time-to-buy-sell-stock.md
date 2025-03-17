# Best Time to Buy and Sell Stock

**Difficulty**: Easy  
**Tags**: Array, Dynamic Programming

## Question
You are given an array `prices` where `prices[i]` is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

## Example Input/Output
**Input**: prices = [7,1,5,3,6,4]  
**Output**: 5  
**Explanation**: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Input**: prices = [7,6,4,3,1]  
**Output**: 0  
**Explanation**: In this case, no transactions are done and the max profit = 0.

## Solution Algorithm
1. Initialize variables to track the minimum price seen so far and the maximum profit
2. Iterate through the array of prices:
   - Update the minimum price if the current price is lower
   - Calculate the potential profit if we sell at the current price
   - Update the maximum profit if the potential profit is higher
3. Return the maximum profit

## Solution Code
```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length <= 1) {
        return 0;
    }
    
    int minPrice = prices[0];
    int maxProfit = 0;
    
    for (int i = 1; i < prices.length; i++) {
        // Update minimum price if current price is lower
        minPrice = Math.min(minPrice, prices[i]);
        
        // Calculate potential profit if we sell at current price
        int potentialProfit = prices[i] - minPrice;
        
        // Update maximum profit if potential profit is higher
        maxProfit = Math.max(maxProfit, potentialProfit);
    }
    
    return maxProfit;
}
``` 