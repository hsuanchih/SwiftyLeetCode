
### Best Time to Buy and Sell Stock III

Say you have an array for which the *ith* element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete at most *two* transactions..

__Note:__ You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

__Example 1:__
```
Input: [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
             Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```
__Example 2:__
```
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```
__Example 3:__
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

### Solution
__O(2\*prices^2) Time, O(prices\*3) Space:__
```Swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var minPrice = Int.max
        var profit : [[Int]] = Array(repeating: Array(repeating: 0, count: prices.count), count: 2+1)
        for transaction in 1..<profit.count {
            for day in 0..<prices.count {
                switch (day, transaction) {
                    
                    // If we're on day 0, the only maximum profit is selling & buying
                    // on the same day
                    case (0, _):
                    if transaction == 1 {
                        minPrice = min(minPrice, prices[day])
                    }
                    
                    // If we're only executing 1 transaction, we want to find the maximum
                    // profit we can make overall (see Best Time to Buy and Sell Stock I)
                    case (_, 1):
                    minPrice = min(minPrice, prices[day])
                    profit[transaction][day] = max(prices[day]-minPrice, profit[transaction][day-1])
                    
                    // Otherwise the maximum profit could be either:
                    // 1. Not selling the stock on that day
                    // 2. Selling the stock on that day
                    // We want to make the optimum choice
                    default:
                    profit[transaction][day] = max(
                        profit[transaction][day-1],
                        prices[day]+maxProfitFor(transaction-1, to: day-1, prices, profit)
                    )
                }
            }
        }
        return profit.last?.last ?? 0
    }
    
    // Helper method to compute the maximum profit up to a certain day for n transactions
    func maxProfitFor(_ transaction: Int, to day: Int, _ prices: [Int], _ profit: [[Int]]) -> Int {
        var result = Int.min
        for i in 0...day {
            result = max(result, profit[transaction][i]-prices[i])
        }
        return result
    }
}
```
__O(2\*prices) Time, O(prices\*4) Space:__
```Swift
class Solution {
    
    var minPrice = Int.max
    var profit : [[Int]] = []
    var prices : [Int] = []
    
    func maxProfit(_ prices: [Int]) -> Int {
        self.prices = prices
        profit = Array(repeating: Array(repeating: 0, count: prices.count), count: 2+1)
        for transaction in 1..<profit.count {
            for day in 0..<prices.count {
                switch (day, transaction) {
                    
                    // If we're on day 0, the only maximum profit is selling & buying
                    // on the same day
                    case (0, _):
                    if transaction == 1 {
                        minPrice = min(minPrice, prices[day])
                    }
                    
                    // If we're only executing 1 transaction, we want to find the maximum
                    // profit we can make overall (see Best Time to Buy and Sell Stock I)
                    case (_, 1):
                    minPrice = min(minPrice, prices[day])
                    profit[transaction][day] = max(prices[day]-minPrice, profit[transaction][day-1])
                    
                    // Otherwise the maximum profit could be either:
                    // 1. Not selling the stock on that day
                    // 2. Selling the stock on that day
                    // We want to make the optimum choice
                    default:
                    profit[transaction][day] = max(
                        profit[transaction][day-1],
                        prices[day]+maxProfitOneTransaction(to: day-1)
                    )
                }
            }
        }
        return profit.last?.last ?? 0
    }
    
    // Helper method to compute the maximum profit up to a certain day for 1 transaction
    var maxProfit : Int = Int.min
    func maxProfitOneTransaction(to day: Int) -> Int {
        maxProfit = max(maxProfit, profit[1][day]-prices[day])
        return maxProfit
    }
}
```