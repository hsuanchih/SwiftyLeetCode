
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

__Constraints:__
* `1 <= prices.length <= 10^5`
* `0 <= prices[i] <= 10^4`

### Solution
__O(pow(prices, 3)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var result: Int = .min
        for buy in 0 ..< prices.count {
            for sell in buy ..< prices.count {
                result = max(result, maxProfit(prices, buy ... sell))
            }
        }
        return result
    }

    func maxProfit(_ prices: [Int], _ transactionRange: ClosedRange<Int>) -> Int {
        var result: Int = .min
        let buy = transactionRange.lowerBound
        for sell in transactionRange {
            result = max(result, prices[sell] - prices[buy])
        }
        return result
    }
}
```
__O(pow(prices, 2)) Time, O(1) Space - Brute-Force Improved:__
```Swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard !prices.isEmpty else { return 0 }
        var maxProfit: Int = 0
        for buy in 0 ..< prices.count - 1 {
            for sell in buy + 1 ..< prices.count where prices[sell] - prices[buy] > maxProfit {
                maxProfit = prices[sell] - prices[buy]
            }
        }
        return maxProfit
    }
}
```
__O(prices) Time, O(1) Space - Greedy:__
```Swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard !prices.isEmpty else { return 0 }
        var minBuyPrice: Int = prices.first!
        var maxProfit: Int = 0

        // Update the lowest buying price as we iterate through prices
        // We can sell on the same day or any day later, so we can safely
        // assume that any price minus the lowest buying price is a valid transaction
        for price in prices {
            maxProfit = max(maxProfit, price - minBuyPrice)
            minBuyPrice = min(minBuyPrice, price)
        }
        return maxProfit
    }
}
```