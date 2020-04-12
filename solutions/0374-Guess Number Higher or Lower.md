
### Guess Number Higher or Lower

We are playing the Guess Game. The game is as follows:

I pick a number from __1__ to __n__. You have to guess which number I picked.</br>
Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API `guess(int num)` which returns 3 possible results `(-1, 1, or 0)`:
```
-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!
```

__Example :__
```
Input: n = 10, pick = 6
Output: 6
```

### Solution
__O(log\[2\](n)):__
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
        var start = 0, end = n, result = -1
        while start+1 < end {
            let mid = start + (end-start)/2
            switch guess(mid) {
                case 0:
                return mid
                case -1:
                end = mid
                case 1:
                start = mid
                default:
                fatalError()
            }
        }
        return guess(start) == 0 ? start : end
    }
}
```