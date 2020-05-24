
### Can I Win

In the "100 game," two players take turns adding, to a running total, any integer from 1..10. The player who first causes the running total to reach or exceed 100 wins.

What if we change the game so that players cannot re-use integers?

For example, two players might take turns drawing from a common pool of numbers of 1..15 without replacement until they reach a total >= 100.

Given an integer `maxChoosableInteger` and another integer `desiredTotal`, determine if the first player to move can force a win, assuming both players play optimally.

You can always assume that `maxChoosableInteger` will not be larger than 20 and `desiredTotal` will not be larger than 300.

__Example__
```
Input:
maxChoosableInteger = 10
desiredTotal = 11

Output:
false

Explanation:
No matter which integer the first player choose, the first player will lose.
The first player can choose an integer from 1 up to 10.
If the first player choose 1, the second player can only choose integers from 2 up to 10.
The second player will win by choosing 10 and get a total = 11, which is >= desiredTotal.
Same with other integers chosen by the first player, the second player will always win.
```

### Solution
```Swift
class Solution {
    #if false
    func canIWin(_ maxChoosableInteger: Int, _ desiredTotal: Int) -> Bool {
        guard desiredTotal != 0 else { return true } 
        var used : [Bool] = Array(repeating: false, count: maxChoosableInteger+1)
        return play(desiredTotal, used)
    }
    
    func play(_ desiredTotal: Int, _ used: [Bool]) -> Bool {
        guard desiredTotal > 0 else { return false }
        var used = used
        for i in 1..<used.count {
            guard !used[i] else { continue }
            used[i] = true
            if !play(desiredTotal-i, used) {
                return true
            }
            used[i] = false
        }
        return false
    }
}
```