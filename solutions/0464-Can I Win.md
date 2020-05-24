
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
__O(maxChoosableInteger!) Time, O(maxChoosableInteger) Space - Minimax:__
```Swift
class Solution {
    func canIWin(_ maxChoosableInteger: Int, _ desiredTotal: Int) -> Bool {
        
        // If the sum of values from 1...maxChoosableInteger is less than desiredTotal,
        // nobody can win
        guard (1+maxChoosableInteger)*maxChoosableInteger/2 >= desiredTotal else { return false }
        
        // If desiredTotal is 0, the first player wins 
        guard desiredTotal != 0 else { return true }
        
        // Keep track of numbers used & start playing
        var used : [Bool] = Array(repeating: false, count: maxChoosableInteger+1)
        return play(desiredTotal, used)
    }
    
    func play(_ desiredTotal: Int, _ used: [Bool]) -> Bool {
        
        // If the player enters a round with desiredTotal <= 0,
        // the player loses
        guard desiredTotal > 0 else { return false }
        
        var used = used
        
        // Iterate through numbers from 1...maxChoosableInteger
        for i in 1..<used.count {
            
            // Skip numbers that are already used
            guard !used[i] else { continue }
            
            // Use all unused numbers, but mark them as used for
            // the next round
            used[i] = true
            
            // If the next player can lose with any of the numbers
            // used here, then the current player can win
            if !play(desiredTotal-i, used) {
                return true
            }
            
            // Mark the current number as unused
            used[i] = false
        }
        return false
    }
}
```
__O(maxChoosableInteger!) Time, O(1) Space - Minimax Optimized:__
```Swift
class Solution {
    func canIWin(_ maxChoosableInteger: Int, _ desiredTotal: Int) -> Bool {

    	// Same logic as the above solution, but use bitmap to track numbers used
    	guard (1+maxChoosableInteger)*maxChoosableInteger/2 >= desiredTotal else { return false }
        guard desiredTotal != 0 else { return true }
        return play(maxChoosableInteger, desiredTotal, 0)
    }
    
    func play(_ maxChoosableInteger: Int, _ desiredTotal: Int, _ used: Int) -> Bool {
    	guard (1+maxChoosableInteger)*maxChoosableInteger/2 >= desiredTotal else { return false }
        guard desiredTotal > 0 else { return false }
        for i in 1...maxChoosableInteger {
            if used & (1 << i) != 0 {
                continue
            }
            if !play(maxChoosableInteger, desiredTotal-i, used | (1 << i)) {
                return true
            }
        }
        return false
    }
}
```
__O(maxChoosableInteger*(2^maxChoosableInteger)) Time, O(2^maxChoosableInteger) Space - Memoization:__
```Swift
class Solution {
    func canIWin(_ maxChoosableInteger: Int, _ desiredTotal: Int) -> Bool {
        guard (1+maxChoosableInteger)*maxChoosableInteger/2 >= desiredTotal else { return false }
        guard desiredTotal != 0 else { return true }
        var memo : [Bool?] = Array(repeating: nil, count: 1 << (maxChoosableInteger+1))
        return play(maxChoosableInteger, desiredTotal, 0, &memo)
    }
    
    func play(_ maxChoosableInteger: Int, _ desiredTotal: Int, _ used: Int, _ memo: inout [Bool?]) -> Bool {
        guard desiredTotal > 0 else { return false }
        if let result = memo[used] {
            return result
        }
        for i in 1...maxChoosableInteger {
            if used & (1 << i) != 0 {
                continue
            }
            if !play(maxChoosableInteger, desiredTotal-i, used | (1 << i), &memo) {
                memo[used] = true
                return true
            }
        }
        memo[used] = false
        return false
    }
}
```