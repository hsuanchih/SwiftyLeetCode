
### Guess Number Higher or Lower

We are playing the Guess Game. The game is as follows:

I pick a number from `1` to `n`. You have to guess which number I picked.</br>
Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API `guess(int num)` which returns 3 possible results:
* `-1`: Your guess is higher than the number I picked (i.e. `num > pick`).
* `1`: Your guess is lower than the number I picked (i.e. `num < pick`).
* `0`: Your guess is equal to the number I picked (i.e. `num == pick`).

Return _the number that I picked._

__Example 1:__
```
Input: n = 10, pick = 6
Output: 6
```

__Example 2:__
```
Input: n = 1, pick = 1
Output: 1
```

__Example 3:__
```
Input: n = 2, pick = 1
Output: 1
```

__Constraints:__
* `1 <= n <= 23^1 - 1`
* `1 <= pick <= n`


### Solution
__O(log\[2\](n)) Time:__
```Swift
/** 
 * Forward declaration of guess API.
 * @param  num -> your guess number
 * @return       -1 if the picked number is lower than your guess number
 *                1 if the picked number is higher than your guess number
 *               otherwise return 0
 * func guess(_ num: Int) -> Int 
 */

class Solution : GuessGame {
    func guessNumber(_ n: Int) -> Int {
        var start: Int = 1, end: Int = n
        while start + 1 < end {
            let mid = start + (end - start) / 2
            switch guess(mid) {
            case 0:
                return mid
            case 1:
                start = mid
            case -1:
                end = mid
            default:
                fatalError()
            }
        }
        return guess(start) == 0 ? start : end
    }
}
```