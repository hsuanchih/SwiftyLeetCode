
### Best Time to Buy and Sell Stock II

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the ith day.

On each day, you may decide to buy and/or sell the stock. You can only hold __at most one__ share of the stock at any time. However, you can buy it then immediately sell it on the __same day__.

Find and return the __maximum profit__ you can achieve.

__Example 1:__
```
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
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

__Constraints:__
* `1 <= prices.length <= 3 * 10^4`
* `0 <= prices[i] <= 10^4`

### Solution
__O(2^prices) Time, Brute Forece - TLE:__
```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard !prices.isEmpty else { return 0 }
        return transact(day: 0, buyPrice: Int.max, prices: prices)
    }
    
    func transact(day: Int, buyPrice: Int, prices: [Int]) -> Int {
        let price = prices[day]
        // We've reached the last day
        if day == prices.count - 1 {
            if buyPrice == Int.max {
                // If we haven't bought the stock (buyPrice == Int.max), 
                // then we've got nothing to sell.
                // Profit on the last day will be 0
                return 0
            } else {
                // If we already bought the stock (buyPrice != Int.max), 
                // we only want to sell if we can make a profit
                return max(price - buyPrice, 0)
            }
        }
        
        if buyPrice == Int.max {
            // If we haven't bought the stock (buyPrice == Int.max), then we can either:
            // 1. Buy the stock today, or
            // 2. Do not buy the stock today
            return max(
                transact(day: day+1, buyPrice: price, prices: prices),
                transact(day: day+1, buyPrice: buyPrice, prices: prices)
            )
        } else {
            // If we already bought the stock (buyPrice != Int.max), then we can either:
            // 1. Sell the stock today, or
            // 2. Do not sell the stock today
            return max(
                transact(day: day+1, buyPrice: Int.max, prices: prices) + price - buyPrice,
                transact(day: day+1, buyPrice: buyPrice, prices: prices)
            )
        }
    }
}
```
__O(2\*prices) Time, O(2\*prices) Space - Bottom-Up Recursion, Top-Down Memoization:__
```swift
class Solution {
    enum Mode {
        case buy, sell
    }
    
    func maxProfit(_ prices: [Int]) -> Int {
        var memo: [Mode: [Int?]] = [
            .buy: Array(repeating: nil, count: prices.count),
            .sell: Array(repeating: nil, count: prices.count)
        ]
        return transact(day: 0, mode: .buy, prices: prices, memo: &memo)
    }
    
    func transact(day: Int, mode: Mode, prices: [Int], memo: inout [Mode: [Int?]]) -> Int {
        guard day < prices.count else { return 0 }
        
        if let value = memo[mode]?[day] {
            return value
        }
        
        let price = prices[day]
        
        switch mode {
        case .buy:
            // When in buy mode, we can either:
            // 1. Buy on another day, or
            // 2. Buy today
            memo[mode]![day] = max(
                transact(day: day+1, mode: .buy, prices: prices, memo: &memo),
                -price + transact(day: day+1, mode: .sell, prices: prices, memo: &memo)
            )
        case .sell:
            // When in sell mode, we can either:
            // 1. Sell on another day, or
            // 2. Sell today
            memo[mode]![day] = max(
                transact(day: day+1, mode: mode, prices: prices, memo: &memo),
                price + transact(day: day+1, mode: .buy, prices: prices, memo: &memo)
            )
        }
        return memo[mode]![day]!
    }
}
```

__O(prices) Time, O(1) Space - Greedy:__
```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var buyPrice: Int = 0
        var maxProfit: Int = 0
        for price in prices {
            // If the current price is more than the buy price:
            // Sell the stock at the current price to profit (price - buyPrice), and
            // buy the stock at the current price again
            
            // If the current price is less than the buy price:
            // Sell the stock at the buy price to avoid losing money (profit 0), and
            // buy the stock at the current price (so we can profit if the price goes up)
            maxProfit += max(0, price - buyPrice)
            buyPrice = price
        }
        return maxProfit - (prices.first ?? 0)
    }
}
```