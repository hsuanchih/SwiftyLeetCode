
### Stone Game

Alex and Lee play a game with piles of stones.</br>
There are an even number of piles __arranged in a row__, and each pile has a positive integer number of stones `piles[i]`.

The objective of the game is to end with the most stones.  The total number of stones is odd, so there are no ties.

Alex and Lee take turns, with Alex starting first.</br>
Each turn, a player takes the entire pile of stones from either the beginning or the end of the row.</br>
This continues until there are no more piles left, at which point the person with the most stones wins.

Assuming Alex and Lee play optimally, return `True` if and only if Alex wins the game.

__Example 1:__
```
Input: [5,3,4,5]
Output: true
Explanation: 
Alex starts first, and can only take the first 5 or the last 5.
Say he takes the first 5, so that the row becomes [3, 4, 5].
If Lee takes 3, then the board is [4, 5], and Alex takes 5 to win with 10 points.
If Lee takes the last 5, then the board is [3, 4], and Alex takes 4 to win with 9 points.
This demonstrated that taking the first 5 was a winning move for Alex, so we return true.
```

__Note:__
1. `2 <= piles.length <= 500`
2. `piles.length` is even.
3. `1 <= piles[i] <= 500`
4. `sum(piles)` is odd.

### Solution
__O(2^n):__
```Swift
class Solution {
    func stoneGame(_ piles: [Int]) -> Bool {
        return play(piles, 0, piles.count-1, 0) > 0
    }
    
    // Simulate each round of gameplay with piles
    // Every pile the opponent earns is deducted from Alex's pile
    // In the end we want to know if Alex's total is greater than his opponent's (ie, result > 0)
    func play(_ piles: [Int], _ start: Int, _ end: Int, _ round: Int) -> Int {

        // If there is only one element remaining, the only choice is to pick the element
        // The opponent will always pick last
        if start == end {
            return -piles[start]
        }

        // If Alex is playing, his goal is to maximize his pile
        // If the opponent is playing, the goal is to minimize Alex's pile
        // Either can pick from the start or the end of the remaining pile
        if round%2 == 0 {
            return max(
                play(piles, start+1, end, round+1) + piles[start],
                play(piles, start, end-1, round+1) + piles[end]
            )
        } else {
            return min(
                play(piles, start+1, end, round+1) - piles[start],
                play(piles, start, end-1, round+1) - piles[end]
            )
        }
    }
}
```
__O(n^2):__
```Swift
class Solution {
    func stoneGame(_ piles: [Int]) -> Bool {
        var memo : [[Int?]] = Array(repeating: Array(repeating: nil, count: piles.count), count: piles.count)
        return winning(piles, 0, piles.count-1, &memo) > 0
    }
    
    func winning(_ piles: [Int], _ start: Int, _ end: Int, _ memo: inout [[Int?]]) -> Int {
        if let value = memo[start][end] {
            return value
        }
        switch end-start {
            case 0:
            return piles[start]
            default:
            memo[start][end] = max(piles[start]-winning(piles, start+1, end, &memo), piles[end]-winning(piles, start, end-1, &memo))
            return memo[start][end]!
        }
    }
}
```