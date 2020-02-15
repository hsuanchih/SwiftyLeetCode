
### Best Time to Buy and Sell Stock

Say you have an array for which the *ith* element is the price of a given stock on day *i*.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

__Example 1:__
```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```
__Example 2:__
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

### Solution
__O(n^2):__
```Swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var result = Int.min
        for buy in stride(from: 0, to: prices.count, by: 1) {
            var profit = -prices[buy]
            for sell in stride(from: buy+1, to: prices.count, by: 1) {
                result = max(result, profit+prices[sell])
            }
        }
        return max(result, 0)
    }
}
```
__O(n):__
```Swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var lowest = Int.max, maxProfit = 0
        for price in prices {
            lowest = min(price, lowest)
            maxProfit = max(maxProfit, price-lowest)
        }
        return maxProfit
    }
}
```