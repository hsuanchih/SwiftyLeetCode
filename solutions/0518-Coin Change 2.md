
### Coin Change 2

You are given an integer array coins representing `coins` of different denominations and an integer `amount` representing a total amount of money.

Return __the number of combinations that make up that amount__. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is __guaranteed__ to fit into a signed __32-bit__ integer.

__Example 1:__
```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```
__Example 2:__
```
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```
__Example 3:__
```
Input: amount = 10, coins = [10]
Output: 1
```

__Constraints:__
* `1 <= coins.length <= 300`
* `1 <= coins[i] <= 5000`
* All the values of `coins` are __unique__.
* `0 <= amount <= 5000`

### Solution
__O(coins^amount) - Top-Down Recursive, TLE:__
```Swift
class Solution {
    func change(_ amount: Int, _ coins: [Int]) -> Int {
        var ways: Int = 0
        exhaustiveSearch(0, coins, amount, &ways)
        return ways
    }
    
    func exhaustiveSearch(_ index: Int, _ coins: [Int], _ amount: Int, _ ways: inout Int) {
        guard amount > 0 else {
            if amount == 0 {
                ways += 1
            }
            return
        }
        
        for i in index ..< coins.count {
            exhaustiveSearch(i, coins, amount - coins[i], &ways)
        }
    }
}
```