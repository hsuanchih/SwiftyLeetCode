
### Best Time to Buy and Sell Stock with Cooldown

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

* You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
* After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

__Example:__
```
Input: [1,2,3,0,2]
Output: 3 
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

### Solution
__O(5^prices) Time, O(1) Space - Bottom-Up Recursive:__
```Swift
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
__O(3\*prices) Time, O(3\*prices) Space - Bottom-Up Recursive, Top-Down Memoization:__
```Swift
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