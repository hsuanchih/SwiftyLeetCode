
### Coin Change

You are given coins of different denominations and a total amount of money *amount*.</br> 
Write a function to compute the fewest number of coins that you need to make up that amount.</br> 
If that amount of money cannot be made up by any combination of the coins, return `-1`.

__Example 1:__
```
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
```
__Example 2:__
```
Input: coins = [2], amount = 3
Output: -1
```

__Note:__
You may assume that you have an infinite number of each kind of coin.

### Solution
__O(coins^amount) Recursive Top-Down:__
```Swift
class Solution {
    func coinChange(_ coins: [Int], _ amount: Int) -> Int {
        return findChange(coins, amount) ?? -1
    }
    
    func findChange(_ coins: [Int], _ amount: Int) -> Int? {
        if amount == 0 {
            return 0
        }
        var minCoins = Int.max
        for coin in coins where amount >= coin {
            minCoins = min(minCoins, findChange(coins, amount-coin) ?? Int.max)
        }
        return minCoins == Int.max ? nil : minCoins+1
    }
}
```
__O(coins*amount) Recursive Top-Down:__
```Swift
class Solution {
    func coinChange(_ coins: [Int], _ amount: Int) -> Int {
        var memo : [Int?] = Array(repeating: nil, count: amount+1)
        let result = findChange(coins, amount, &memo)
        return result == Int.max ? -1 : result
    }
    
    func findChange(_ coins: [Int], _ amount: Int, _ memo: inout [Int?]) -> Int {
        if amount == 0 {
            memo[amount] = 0
        }
        if let result = memo[amount] {
            return min(result, Int.max)
        }
        var minCoins = Int.max
        for coin in coins where amount >= coin {
            minCoins = min(minCoins, findChange(coins, amount-coin, &memo))
        }
        memo[amount] = min(minCoins, Int.max-1)+1
        return memo[amount]!
    }
}
```
__O(coins*amount) Iterative Bottom-Up:__
```Swift
class Solution {
    func coinChange(_ coins: [Int], _ amount: Int) -> Int {

        // 0 coins are required to make up for amount 0
        if amount == 0 { return 0 }

        // memo[index][amount] represents the minimum number of coins required to make up amount using coins in 
        // coins[0] up to coins[index]
        var memo : [[Int]] = Array(repeating: Array(repeating: Int.max-1, count: amount+1), count: coins.count)
        
        for (index, coin) in coins.enumerated() {
            for sum in 0...amount {
                switch (index, sum-coin) {

                    // If amount is equal to the coin value, minimum number of coins required to make up amount is 1
                    case (_, 0):
                    memo[index][sum] = 1
                    
                    // This is the first coin in coins, if amount is greater than the coin value, check if we can
                    // make up amount-coin using the same coin. If so, add one more coin to make up amount
                    case (0, 1..<amount):
                    memo[index][sum] = min(memo[index][sum], memo[index][sum-coin]+1)

                    // If the current coin value is larger than the amount we need to make up, such amount can
                    // only come from smaller coin values if any, carry over result from previous
                    // computations from smaller denominations
                    case (1..<coins.count, Int.min..<0):
                    memo[index][sum] = memo[index-1][sum]

                    // We can make up the target amount using either coins from all previous denomination, or
                    // including also the current coin; take the option which requires fewer coins
                    case (1..<coins.count, 1..<amount):
                    memo[index][sum] = min(memo[index][sum-coin]+1, memo[index-1][sum])

                    // This is the case where we're dealing with the first coin in coins, yet this coin value
                    // is larger than the amount we're dealing with. We cannot make up these amounts using any of
                    // the coins we have. Do nothing.
                    default:
                    break
                }
            }
        }
        if let result = memo.last?.last, result != Int.max-1 {
            return result
        }
        return -1
    }
}
```