
### Sum of Root To Leaf Binary Numbers

Alice and Bob take turns playing a game, with Alice starting first.

Initially, there is a number `N` on the chalkboard.  On each player's turn, that player makes a *move* consisting of:

* Choosing any `x` with `0 < x < N` and `N % x == 0`.
* Replacing the number `N` on the chalkboard with `N - x`.
Also, if a player cannot make a move, they lose the game.

Return `True` if and only if Alice wins the game, assuming both players play optimally.

__Example 1:__
```
Input: 2
Output: true
Explanation: Alice chooses 1, and Bob has no more moves.
```
__Example 2:__
```
Input: 3
Output: false
Explanation: Alice chooses 1, Bob chooses 1, and Alice has no more moves.
```

__Note:__
1. `1 <= N <= 1000`

### Solution
__O(N^N):__
```Swift
class Solution {
    func divisorGame(_ N: Int) -> Bool {
        return play(N, 0)
    }

    /// Simulates each round of play, the return value 
    /// represents whether Alice will win
    func play(_ N: Int, _ round: Int) -> Bool {

        // Alice plays even rounds, Bob
        let isAlice = round%2 == 0

        // Base case: whoever ends up with a 1 loses
        // If Alice loses, return false
        // If Bob loses, return true
        if N == 1 {
            return !isAlice
        }

        // Try all x's in 0 < x < N
        for x in 1..<N {

            // If N%x == 0, continue the next round with N-x
            if N%x == 0 {
                let willWin = play(N-x, round+1)

                // Each player plays optimally:
                // If Alice can win game with a certain value of x (willWin == true), 
                // she will go with x
                // If Bob can win game with a certain value of x (willWin == false),
                // he will go with x
                if isAlice && willWin {
                    return true
                }
                if !isAlice && !willWin {
                    return false
                }
            }
        }

        // If Alice cannot find any x to help her win the game starting at N, return false (Bob wins)
        // If Bob cannot find any x to help him win the game starting at N, return true (Alice wins)
        return !isAlice
    }
}
```
__O(N^2) Recursion with Memoization:__
```Swift
class Solution {
    func divisorGame(_ N: Int) -> Bool {
        var memo : [Bool?] = Array(repeating: nil, count: N+1)
        return play(N, 0, &memo)
    }

    /// Simulates each round of play, the return value 
    /// represents whether Alice will win
    func play(_ N: Int, _ round: Int, _ memo: inout [Bool?]) -> Bool {

        // Alice plays even rounds, Bob
        let isAlice = round%2 == 0

        // Base case: whoever ends up with a 1 loses
        // If Alice loses, return false
        // If Bob loses, return true
        if N == 1 {
            return !isAlice
        }

        if let result = memo[N] {
            return result
        }

        // Try all x's in 0 < x < N
        for x in 1..<N {

            // If N%x == 0, continue the next round with N-x
            if N%x == 0 {
                let willWin = play(N-x, round+1, &memo)

                // Each player plays optimally:
                // If Alice can win game with a certain value of x (willWin == true), 
                // she will go with x
                // If Bob can win game with a certain value of x (willWin == false),
                // he will go with x
                switch (isAlice, willWin) {
                    case (true, true):
                    memo[N] = true
                    case (false, false):
                    memo[N] = false
                    default:
                    break
                }
                if let result = memo[N] {
                    return result
                }
            }
        }

        // If Alice cannot find any x to help her win the game starting at N, return false (Bob wins)
        // If Bob cannot find any x to help him win the game starting at N, return true (Alice wins)
        memo[N] = !isAlice
        return memo[N]!
    }
}
```
__O(N^2) Iterative DP:__
```Swift
class Solution {
    func divisorGame(_ N: Int) -> Bool {
        var memo : [Bool] = Array(repeating: false, count: N+1)
        for N in stride(from: 2, to: memo.count, by: 1) {
            for x in stride(from: 1, to: N, by: 1) {
                if N%x == 0 && memo[N-x] == false {
                    memo[N] = true
                    break
                }
            }
        }
        return memo[N]
    }
}
```