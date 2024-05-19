
### Snakes and Ladders

You are given an `n x n` integer matrix `board` where the cells are labeled from `1 to pow(n, 2)` in a Boustrophedon style starting from the bottom left of the board (i.e. `board[n - 1][0]`) and alternating direction each row.

You start on square `1` of the board. In each move, starting from square `curr`, do the following:
* Choose a destination square `next` with a label in the range `[curr + 1, min(curr + 6, pow(n, 2)]`.
    * This choice simulates the result of a standard 6-sided die roll: i.e., there are always at most 6 destinations, regardless of the size of the board.
* If `next` has a snake or ladder, you must move to the destination of that snake or ladder. Otherwise, you move to `next`.
* The game ends when you reach the square `pow(n, 2)`.

A board square on row `r` and column `c` has a snake or ladder if `board[r][c] != -1`. The destination of that snake or ladder is `board[r][c]`. Squares `1` and `pow(n, 2)` do not have a snake or ladder.

Note that you only take a snake or ladder at most once per move. If the destination to a snake or ladder is the start of another snake or ladder, you do not follow the subsequent snake or ladder.
* For example, suppose the board is `[[-1,4],[-1,3]]`, and on the first move, your destination square is `2`. You follow the ladder to square `3`, but do not follow the subsequent ladder to `4`.

Return the least number of moves required to reach the square `pow(n, 2)`. If it is not possible to reach the square, return `-1`.

__Example 1:__
```
Input: board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]
Output: 4
Explanation: 
In the beginning, you start at square 1 (at row 5, column 0).
You decide to move to square 2 and must take the ladder to square 15.
You then decide to move to square 17 and must take the snake to square 13.
You then decide to move to square 14 and must take the ladder to square 35.
You then decide to move to square 36, ending the game.
This is the lowest possible number of moves to reach the last square, so return 4.
```
__Example 2:__
```
Input: board = [[-1,-1],[-1,3]]
Output: 1
```

__Constraints:__
* `n == board.length == board[i].length`
* `2 <= n <= 20`
* `board[i][j]` is `either -1` or in the range `[1, pow(n, 2)]`.
* The squares labeled `1` and `pow(n, 2)` do not have any ladders or snakes.

### Solution
__BFS:__
```Swift
class Solution {
    func snakesAndLadders(_ board: [[Int]]) -> Int {
        guard !board.isEmpty else { return -1 }
        let n: Int = board.count
        let destination: Int = n * n
        var queue: [Int] = [1]
        var seen: Set<Int> = [1]
        var levels: Int = 0

        while !queue.isEmpty {
            let count: Int = queue.count
            for _ in 0 ..< count {
                let index: Int = queue.removeFirst()
                if index >= destination {
                    return levels
                } else {
                    for next in index + 1 ... index + 6 where next <= destination {
                        let (row, col) = coordinate(n, next)
                        let value: Int = board[row][col]
                        switch value {
                        case -1 where !seen.contains(next):
                            seen.insert(next)
                            queue.append(next)
                        case -1:
                            break
                        case let value where !seen.contains(value):
                            seen.insert(value)
                            queue.append(value)
                        default:
                            break
                        }
                    }
                }
            }
            levels += 1
        }
        return -1
    }

    func coordinate(_ n: Int, _ index: Int) -> (Int, Int) {
        let row: Int = (index - 1) / n
        let col: Int = (index - 1) % n
        if row % 2 == 0 {
            return (n - row - 1, col)
        } else {
            return (n - row - 1, n - col - 1)
        }
    }
}
```