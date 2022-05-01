
### Best Time to Buy and Sell Stock with Transaction Fee

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

__Note:__ 
You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

__Example 1:__
```
Input: prices = [1,3,2,8,4,9], fee = 2

Output: 8

Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```
__Example 2:__
```
Input: prices = [1,3,7,5,10,3], fee = 3

Output: 6
```

__Constraints:__
* `1 <= prices.length <= 5 * 10^4`
* `1 <= prices[i] < 5 * 10^4`
* `0 <= fee < 5 * 10^4`

### Solution
__O(2\*prices) Time, O(2\*prices) Space - Bottom-Up Recursion, Top-Down Memoization:__
```swift
class Solution {
    enum Mode {
        case buy, sell
    }
    
    func maxProfit(_ prices: [Int], _ fee: Int) -> Int {
        var memo: [Mode: [Int?]] = [
            .buy: Array(repeating: nil, count: prices.count),
            .sell: Array(repeating: nil, count: prices.count)
        ]
        return transact(day: 0, mode: .buy, prices: prices, fee: fee, memo: &memo)
    }
    
    func transact(day: Int, mode: Mode, prices: [Int], fee: Int, memo: inout [Mode: [Int?]]) -> Int {
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
                transact(day: day+1, mode: .buy, prices: prices, fee: fee, memo: &memo),
                -price + transact(day: day+1, mode: .sell, prices: prices, fee: fee, memo: &memo)
            )
        case .sell:
            // When in sell mode, we can either:
            // 1. Sell on another day, or
            // 2. Sell today
            memo[mode]![day] = max(
                transact(day: day+1, mode: mode, prices: prices, fee: fee, memo: &memo),
                price - fee + transact(day: day+1, mode: .buy, prices: prices, fee: fee, memo: &memo)
            )
        }
        return memo[mode]![day]!
    }
}
```