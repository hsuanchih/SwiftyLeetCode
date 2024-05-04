
### Guess Number Higher or Lower

We are playing the Guess Game. The game is as follows:

I pick a number from `1` to `n`. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API `int guess(int num)`, which returns three possible results:
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
* `1 <= n <= pow(2, 31) - 1`
* `1 <= pick <= n`


### Solution
__O(n) Time: Linear Search__
```Swift
/** 
 * Forward declaration of guess API.
 * @param  num -> your guess number
 * @return 	     -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0 
 * func guess(_ num: Int) -> Int 
 */

class Solution : GuessGame {
    func guessNumber(_ n: Int) -> Int {
        for num in 1 ... n where guess(num) == 0 {
            return num
        }
        fatalError()
    }
}
```
__O(log(n)) Time: Binary Search__
```Swift
/** 
 * Forward declaration of guess API.
 * @param  num -> your guess number
 * @return 	     -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0 
 * func guess(_ num: Int) -> Int 
 */

class Solution : GuessGame {
    func guessNumber(_ n: Int) -> Int {
        var start: Int = 1, end: Int = n
        while start + 1 < end {
            let num: Int = start + (end - start) / 2
            switch guess(num) {
            case 0:
                return num
            case 1:
                start = num
            case -1:
                end = num
            default:
                fatalError()
            }
        }
        if guess(start) == 0 {
            return start
        } else if guess(end) == 0 {
            return end
        } else {
            fatalError()
        }
    }
}
```