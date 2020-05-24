
### Guess Number Higher or Lower II

We are playing the Guess Game. The game is as follows:

I pick a number from __1__ to __n__. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number I picked is higher or lower.

However, when you guess a particular number x, and you guess wrong, you pay __$x__. You win the game when you guess the number I picked.

__Example:__
```
n = 10, I pick 8.

First round:  You guess 5, I tell you that it's higher. You pay $5.
Second round: You guess 7, I tell you that it's higher. You pay $7.
Third round:  You guess 9, I tell you that it's lower. You pay $9.

Game over. 8 is the number I picked.

You end up paying $5 + $7 + $9 = $21.
```
Given a particular __n ≥ 1__, find out how much money you need to have to guarantee a __win__.

### Solution
__Minimax:__
```Swift
class Solution {
    func getMoneyAmount(_ n: Int) -> Int {
        return guess(1, n)
    }
    
    func guess(_ start: Int, _ end: Int) -> Int {
        if start >= end {
            return 0
        }
        var min = Int.max
        for i in start...end {
            min = Swift.min(max(guess(start, i-1), guess(i+1, end))+i, min)
        }
        return min
    }
}
```
__Minimax + Memoization:__
```Swift
class Solution {
    func getMoneyAmount(_ n: Int) -> Int {
        var memo: [[Int?]] = Array(repeating: Array(repeating: nil, count:n+1), count: n+1)
        return guess(1, n, &memo)
    }
    
    func guess(_ start: Int, _ end: Int, _ memo: inout [[Int?]]) -> Int {
        if start >= end {
            return 0
        }
        if let result = memo[start][end] {
            return result
        }
        var min = Int.max
        for i in start...end {
            min = Swift.min(max(guess(start, i-1, &memo), guess(i+1, end, &memo))+i, min)
        }
        memo[start][end] = min
        return min
    }
}
```