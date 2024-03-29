
### Edit Distance

Given two words *word1* and *word2*, find the minimum number of operations required to convert *word1* to *word2*.

You have the following 3 operations permitted on a word:
1. Insert a character
2. Delete a character
3. Replace a character

__Example 1:__
```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```
__Example 2:__
```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

### Solution
__O(pow(3, min(word1, word2))) Time, O(1) Space - Bottom-Up Recursive Brute-Force:__
```Swift
class Solution {
    func minDistance(_ word1: String, _ word2: String) -> Int {
        minD(Array(word1), Array(word2), 0, 0)
    }

    func minD(_ word1: [Character], _ word2: [Character], _ index1: Int, _ index2: Int) -> Int {
        switch (index1, index2) {
        case (word1.count, let index2):
            // If we've reached the end of either word1, 
            // the difference between index2 & length of word2 is the
            // outstanding edit difference that needs to be matched
            return word2.count - index2
        case (let index1, word2.count):
            // If we've reached the end of either word2, 
            // the difference between index1 & length of word1 is the
            // outstanding edit difference that needs to be matched
            return word1.count - index1
        case (let index1, let index2) where word1[index1] == word2[index2]:
            // If the characters match, no edits need to be made
            return minD(word1, word2, index1 + 1, index2 + 1)
        case (let index1, let index2):
            // If the characters don't match, we can:
            // Remove/replace/insert (+1 edit distance) a character in word1 and
            // continue finding the minimum edit distance
            let insert = minD(word1, word2, index1, index2 + 1)
            let delete = minD(word1, word2, index1 + 1, index2)
            let replace = minD(word1, word2, index1 + 1, index2 + 1)
            return 1 + min(min(insert, delete), replace)
        }
    }
}
```
__O(word1\*word2) Time, O(word1\*word2) Space - Bottom-Up Recursive, Top-Down Memoization:__
```Swift
class Solution {
    func minDistance(_ word1: String, _ word2: String) -> Int {
        var memo : [[Int?]] = Array(repeating: Array(repeating: nil, count: word2.count), count: word1.count)
        return minDistance(Array(word1), Array(word2), 0, 0, &memo)
    }
    
    func minDistance(_ word1: [Character], _ word2: [Character], _ i: Int, _ j: Int, _ memo: inout [[Int?]]) -> Int {
        switch (i, j) {
            case (word1.count, _):
            return word2.count-j
            case (_, word2.count):
            return word1.count-i
            default:
            break
        }
        if let result = memo[i][j] {
            return result
        }
        switch word1[i] {
            case word2[j]:
            memo[i][j] = minDistance(word1, word2, i+1, j+1, &memo)
            default:
            memo[i][j] = 1+min(
                minDistance(word1, word2, i+1, j+1, &memo),
                min(minDistance(word1, word2, i+1, j, &memo), minDistance(word1, word2, i, j+1, &memo))
            )
        }
        return memo[i][j]!
    }
}
```
__O(word1\*word2) Time, O(word1\*word2) Space - Bottom-Up Iterative, Bottom-Up Memoization:__
```Swift
class Solution {
    func minDistance(_ word1: String, _ word2: String) -> Int {
        let word1 = Array(word1), word2 = Array(word2)
        var memo : [[Int]] = Array(repeating: Array(repeating: 0, count: word2.count+1), count: word1.count+1)
        for i in 0...word1.count {
            for j in 0...word2.count {
                switch (i, j) {
                    case (0, 0):
                    break
                    case (0, _):
                    memo[i][j] = j
                    case (_, 0):
                    memo[i][j] = i
                    default:
                    if word1[i-1] == word2[j-1] {
                        memo[i][j] = memo[i-1][j-1]
                    } else {
                        memo[i][j] = 1 + min(memo[i-1][j-1], min(memo[i][j-1], memo[i-1][j]))
                    }
                }
            }
        }
        return memo.last?.last ?? 0
    }
}
```