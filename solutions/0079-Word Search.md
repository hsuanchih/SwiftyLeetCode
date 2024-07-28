
### Word Search

Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

__Example 1:__

![question_79-0.jpg](../images/question_79-0.jpg)
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```
__Example 2:__

![question_79-1.jpg](../images/question_79-1.jpg)
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
```
__Example 3:__

![question_79-2.jpg](../images/question_79-2.jpg)
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```

__Constraints:__
* `m == board.length`
* `n = board[i].length`
* `1 <= m, n <= 6`
* `1 <= word.length <= 15`
* `board` and `word` consists of only lowercase and uppercase English letters.

__Follow up:__ 
* Could you use search pruning to make your solution faster with a larger `board`?

### Solution
__O(board * pow(4, word)) Time:__
```Swift
class Solution {
    func exist(_ board: [[Character]], _ word: String) -> Bool {
        let word: [Character] = Array(word)
        var visited: Set<[Int]> = []
        for row in 0 ..< board.count {
            for col in 0 ..< board.first!.count {
                if dfs(board, word, 0, row, col, &visited) {
                    return true
                }
            }
        }
        return false
    }

    func dfs(_ board: [[Character]], _ word: [Character], _ index: Int, _ row: Int, _ col: Int, _ visited: inout Set<[Int]>) -> Bool {
        if index == word.count {
            return true
        } else {
            switch (row, col) {
            case (0 ..< board.count, 0 ..< board.first!.count) where !visited.contains([row, col]) && board[row][col] == word[index]:
                visited.insert([row, col])
                for offset in [-1, 1] where dfs(board, word, index + 1, row + offset, col, &visited) || dfs(board, word, index + 1, row, col + offset, &visited) {
                    return true
                }
                visited.remove([row, col])
                return false
            default:
                return false
            }
        }
    }
}
```