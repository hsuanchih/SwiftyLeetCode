
### Edit Distance

Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`.

You have the following three operations permitted on a word:
* Insert a character
* Delete a character
* Replace a character

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

__Constraints:__
* `0 <= word1.length, word2.length <= 500`
* `word1` and `word2` consist of lowercase English letters.

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
__O(word1 * word2) Time, O(word1 * word2) Space - Bottom-Up Recursive + Memoization:__
```Swift
class Solution {
    func minDistance(_ word1: String, _ word2: String) -> Int {
        var memo: [[Int?]] = Array(repeating: Array(repeating: nil, count: word2.count), count: word1.count)
        return minD(Array(word1), Array(word2), 0, 0, &memo)
    }

    func minD(_ word1: [Character], _ word2: [Character], _ index1: Int, _ index2: Int, _ memo: inout [[Int?]]) -> Int {
        switch (index1, index2) {
        case (word1.count, let index2):
            return word2.count - index2
        case (let index1, word2.count):
            return word1.count - index1
        case (let index1, let index2) where word1[index1] == word2[index2]:
            if let result = memo[index1][index2] {
                return result
            } else {
                memo[index1][index2] = minD(word1, word2, index1 + 1, index2 + 1, &memo)
                return memo[index1][index2]!
            }
        case (let index1, let index2):
            if let result = memo[index1][index2] {
                return result
            } else {
                let insert = minD(word1, word2, index1, index2 + 1, &memo)
                let delete = minD(word1, word2, index1 + 1, index2, &memo)
                let replace = minD(word1, word2, index1 + 1, index2 + 1, &memo)
                memo[index1][index2] = 1 + min(min(insert, delete), replace)
                return memo[index1][index2]!
            }
        }
    }
}
```
__O(word1 * word2) Time, O(word1 * word2) Space - Top-Down Iterative + Memoization:__
```Swift
class Solution {
    func minDistance(_ word1: String, _ word2: String) -> Int {
        let word1: [Character] = Array(word1), word2: [Character] = Array(word2)
        var minD: [[Int]] = Array(repeating: Array(repeating: 0, count: word2.count + 1), count: word1.count + 1)
        for index1 in stride(from: word1.count, through: 0, by: -1) {
            for index2 in stride(from: word2.count, through: 0, by: -1) {
                switch (index1, index2) {
                case (word1.count, let index2):
                    minD[index1][index2] = word2.count - index2
                case (let index1, word2.count):
                    minD[index1][index2] = word1.count - index1
                case (let index1, let index2) where word1[index1] == word2[index2]:
                    minD[index1][index2] = minD[index1 + 1][index2 + 1]
                case (let index1, let index2):
                    let insert = minD[index1][index2 + 1]
                    let delete = minD[index1 + 1][index2]
                    let replace = minD[index1 + 1][index2 + 1]
                    minD[index1][index2] = 1 + min(min(insert, delete), replace)
                }
            }
        }
        return minD.first?.first ?? 0
    }
}
```
__O(word1 * word2) Time, O(word1 * word2) Space - Bottom-Up Iterative + Memoization:__
```Swift
class Solution {
    func minDistance(_ word1: String, _ word2: String) -> Int {
        let word1 = Array(word1), word2 = Array(word2)
        var minD: [[Int]] = Array(repeating: Array(repeating: 0, count: word2.count + 1), count: word1.count + 1)
        for index1 in 0 ... word1.count {
            for index2 in 0 ... word2.count {
                switch (index1, index2) {
                case (0, 0):
                    break
                case (0, let index2):
                    minD[index1][index2] = index2 - index1
                case (let index1, 0):
                    minD[index1][index2] = index1 - index2
                case (let index1, let index2) where word1[index1 - 1] == word2[index2 - 1]:
                    minD[index1][index2] = minD[index1 - 1][index2 - 1]
                case (let index1, let index2):
                    minD[index1][index2] = 1 + min(minD[index1 - 1][index2 - 1], min(minD[index1][index2 - 1], minD[index1 - 1][index2]))
                }
            }
        }
        return minD.last?.last ?? 0
    }
}
```