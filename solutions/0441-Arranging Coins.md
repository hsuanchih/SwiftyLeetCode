
### Arranging Coins

You have `n` coins and you want to build a staircase with these coins. The staircase consists of `k` rows where the `ith` row has exactly `i` coins. The last row of the staircase __may be__ incomplete.

Given the integer `n`, return the number of __complete rows__ of the staircase you will build.

__Example 1:__

![question_441-0.jpg](../images/question_441-0.jpg)
```
Input: n = 5
Output: 2
Explanation: Because the 3rd row is incomplete, we return 2.
```
__Example 2:__

![question_441-1.jpg](../images/question_441-1.jpg)
```
Input: n = 8
Output: 3
Explanation: Because the 4th row is incomplete, we return 3.
```

__Constraints:__
* `1 <= n <= pow(2, 31) - 1`

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
__O(n) Time:__
```Swift
class Solution {
    func arrangeCoins(_ n: Int) -> Int {
        for k in 1 ... n {
            let coins: Int = (1 + k) * k / 2
            if coins == n {
                return k
            } else if coins > n {
                return k - 1
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
            case ..<n:
                start = mid
            case (n + 1)...:
                end = mid
            default:
                fatalError()
            }
        }
        return (1 + end) * end / 2 <= n ? end : start
    }
}
```