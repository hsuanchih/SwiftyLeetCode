
### Nim Game

You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

__Example:__
```
Input: 4
Output: false 
Explanation: If there are 4 stones in the heap, then you will never win the game;
             No matter 1, 2, or 3 stones you remove, the last stone will always be 
             removed by your friend.
```

### Solution
__O(3^n) Time, O(1) Space - Minimax:__
```Swift
class Solution {
    func canWinNim(_ n: Int) -> Bool {
        return play(n)
    }
    
    func play(_ n: Int) -> Bool {

        // The player to play the round entered with no stone loses the game
        guard n > 0 else { return false }

        // Try removing 1...3 stones in each round
        // If any of these moves result in the next player losing
        // in the end, the current player has the chance to win
        for i in 1...3 {
            if !play(n-i) {
                return true
            }
        }
        return false
    }
}
```
__O(n) Time, O(n) Space - Top-Down Recursive, Bottom-Up Memoization:__
```Swift
class Solution {
    func canWinNim(_ n: Int) -> Bool {
        var memo : [Bool?] = Array(repeating: nil, count: n+1)
        return play(n, &memo)
    }
    
    func play(_ n: Int, _ memo: inout [Bool?]) -> Bool {
        guard n > 0 else { return false }
        if let result = memo[n] {
            return result
        }
        for i in 1...3 {
            if !play(n-i, &memo) {
                memo[n] = true
                break
            }
        }
        if memo[n] == nil {
            memo[n] = false
        }
        return memo[n]!
    }
}
```
__O(1) Time, O(1) Space - Math:__
```Swift
class Solution {
    func canWinNim(_ n: Int) -> Bool {
        return !(n%4 == 0)
    }
}
```