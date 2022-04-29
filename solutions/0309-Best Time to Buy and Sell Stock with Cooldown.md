
### Best Time to Buy and Sell Stock with Cooldown

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

* After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
__Note:__ You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

__Example 1:__
```
Input: [1,2,3,0,2]
Output: 3 
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```
__Example 2:__
```
Input: prices = [1]
Output: 0
```

__Constraints:__
* `1 <= prices.length <= 5000`
* `0 <= prices[i] <= 1000`

### Solution
__O(5^prices) Time, O(1) Space - Bottom-Up Recursive:__
```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        return maxProfit(prices, 0, true, false)
    }
    
    func maxProfit(_ prices: [Int], _ index: Int, _ canBuy: Bool, _ canSell: Bool) -> Int {
        if index == prices.count {
            return 0
        }

        // At each index we face one of the following scenarios:
        // 1. We currently don't hold a stock, so we can choose to buy at the current price or not
        // 2. We currently hold a stock, and we can choose to either choose to sell at the current price or not
        // 3. We've sold a stock the day before, so we cannot exercise a buy option on the current day
        switch (canBuy, canSell) {
            case (true, _):
            return max(
                -prices[index]+maxProfit(prices, index+1, false, true),
                maxProfit(prices, index+1, true, false)
            )
            case (_, true):
            return max(
                maxProfit(prices, index+1, false, true),
                prices[index]+maxProfit(prices, index+1, false, false)
            )
            default:
            return maxProfit(prices, index+1, true, false)
        }
    }
}
```
__O(2^prices) Time, O(1) Space - TLE:__
```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard !prices.isEmpty else { return 0 }
        return transact(day: 0, buyPrice: Int.max, prices: prices)
    }
    
    func transact(day: Int, buyPrice: Int, prices: [Int]) -> Int {
        guard day < prices.count else { return 0 }
        
        let price = prices[day]
        // We've reached the last day
        if day == prices.count - 1 {
            // * If we've bought the stock & haven't sold, we want to sell it only if
            //   we don't lose money doing so. 
            //   Return max(price - buyPrice, 0)
            // * If we haven't bought the stock (buyPrice == Int.max), then we've got nothing to sell.
            //   Return 0
            return max(price - buyPrice, 0)
        }
        
        return max(
            // We can sell the stock on this day, from which we profit (price - buyPrice),
            // and go into the next day with no stocks to sell
            transact(day: day+2, buyPrice: Int.max, prices: prices) + price - buyPrice,
            // We can also not sell the stock on this day, so the profit is 0.
            // But if the price today is lower than the price at which we bought the stock,
            // the optmial strategy is to buy & sell the stock at the same price on the same day
            // and buy the lower price today
            transact(day: day+1, buyPrice: min(buyPrice, price), prices: prices)
        )
    }
}
```
__O(3\*prices) Time, O(3\*prices) Space - Bottom-Up Recursive, Top-Down Memoization:__
```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        var memo : [[Int?]] = Array(repeating: Array(repeating: nil, count: prices.count), count: 3)
        return maxProfit(prices, 0, true, false, &memo)
    }
    
    func maxProfit(_ prices: [Int], _ index: Int, _ canBuy: Bool, _ canSell: Bool, _ memo: inout [[Int?]]) -> Int {
        if index == prices.count {
            return 0
        }
        switch (canBuy, canSell) {
            case (true, _):
            if let result = memo[0][index] {
                return result
            }
            memo[0][index] = max(
                -prices[index]+maxProfit(prices, index+1, false, true, &memo),
                maxProfit(prices, index+1, true, false, &memo)
            )
            return memo[0][index]!
            case (_, true):
            if let result = memo[1][index] {
                return result
            }
            memo[1][index] = max(
                maxProfit(prices, index+1, false, true, &memo),
                prices[index]+maxProfit(prices, index+1, false, false, &memo)
            )
            return memo[1][index]!
            default:
            if let result = memo[2][index] {
                return result
            }
            memo[2][index] = maxProfit(prices, index+1, true, false, &memo)
            return memo[2][index]!
        }
    }
}
```