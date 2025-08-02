
### Coin Change

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the __fewest__ number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

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
__Example 3:__
```
Input: coins = [1], amount = 0
Output: 0
```
__Example 4:__
```
Input: coins = [1, 3, 4], amount = 6
Output: 2
Explanation: 6 = 3 + 3
```

__Constraints:__
* `1 <= coins.length <= 12`
* `1 <= coins[i] <= pow(2, 31) - 1`
* `0 <= amount <= pow(10, 4)`

### Solution
__O(pow(coins, amount)) - Top-Down Recursive:__
```Swift
class Solution {
    func coinChange(_ coins: [Int], _ amount: Int) -> Int {
        // If we don't find a solution with the coins we have making up to the current amount,
        // return -1
        minCoins(coins, amount) ?? -1
    }

    func minCoins(_ coins: [Int], _ amount: Int) -> Int? {
        if amount == 0 {
            // Base case:
            // If we eventually reach amount 0 exactly, it means there
            // is a valid solution - return 0
            return 0
        } else {
            // We want to try all possible coins that are smaller
            // than the current amount to find the minimum number of
            // coins we need to make up the current amount
            // Also, the conditional check guarantees that the amount will never
            // go below zero
            var minCoinsForAmount: Int = .max
            for coin in coins where amount >= coin {
                minCoinsForAmount = min(minCoinsForAmount, minCoins(coins, amount - coin) ?? .max)
            }
            return minCoinsForAmount == .max ? nil : minCoinsForAmount + 1
        }
    }
}
```
__O(coins*amount) - Top-Down Recursive, Bottom-Up Memoization:__
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
__O(coins*amount) Bottom-Up Iterative, Bottom-Up Memoization:__
```Swift
class Solution {
    func coinChange(_ coins: [Int], _ amount: Int) -> Int {
        var memo : [Int] = Array(repeating: amount+1, count: amount+1)
        memo[0] = 0
        for coin in coins {
            for sum in stride(from: 0, through: amount, by: 1) where sum-coin >= 0 {
                memo[sum] = min(memo[sum], memo[sum-coin]+1)
            }
        }
        return memo.last! > amount ? -1 : memo.last!
    }
}
```