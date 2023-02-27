
### Arranging Coins

You have `n` coins and you want to build a staircase with these coins. The staircase consists of `k` rows where the `ith` row has exactly `i` coins. The last row of the staircase __may be__ incomplete.

Given the integer `n`, return _the number of complete rows of the staircase you will build_.

__Example 1:__
```
n = 5

The coins can form the following rows:
¤
¤ ¤
¤ ¤

Because the 3rd row is incomplete, we return 2.
```
__Example 2:__
```
n = 8

The coins can form the following rows:
¤
¤ ¤
¤ ¤ ¤
¤ ¤

Because the 4th row is incomplete, we return 3.
```

__Constraints:__
* `1 <= n <= 2^31 - 1`

### Solution
__O(n) Time:__
```Swift
class Solution {
    func arrangeCoins(_ n: Int) -> Int {
        var coins: Int = 0
        for row in 1 ... n {
            coins += row
            switch coins {
            case n:
                return row
            case n + 1 ... Int.max:
                return row - 1
            default:
                break
            }
        }
        fatalError()
    }
}
```
__O(log(n)) Time:__
```Swift
class Solution {
    func arrangeCoins(_ n: Int) -> Int {
        var start: Int = 1, end: Int = n
        while start + 1 < end {
            let mid: Int = start + (end - start) / 2
            switch (1 + mid) * mid / 2 {
            case n:
                return mid
            case n + 1 ... Int.max:
                end = mid
            default:
                start = mid
            }
        }
        return (1 + end) * end / 2 <= n ? end : start
    }
}
```