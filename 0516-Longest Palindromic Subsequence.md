
### Longest Palindromic Subsequence

Given a string `s`, find the longest palindromic subsequence's length in `s`. You may assume that the maximum length of `s` is `1000`.

__Example 1:__
```
Input:

"bbbab"
Output:
4
One possible longest palindromic subsequence is "bbbb".
```
__Example 2:__
```
Input:

"cbbd"
Output:
2
One possible longest palindromic subsequence is "bb".
```

### Solution
__Recursive O(2^s) Time, O(1) Space:__
```Swift
class Solution {
    func longestPalindromeSubseq(_ s: String) -> Int {
        return longest(Array(s), 0, s.count-1)
    }
    
    func longest(_ s: [Character], _ start: Int, _ end: Int) -> Int {
        switch start {
            case end+1...s.count:
            return 0
            case end:
            return 1
            default:
            break
        }
        if s[start] == s[end] {
            return longest(s, start+1, end-1) + 2
        }
        return max(longest(s, start+1, end), longest(s, start, end-1))
    }
}
```
__Recursive O(s^2) Time, O(s^2) Space:__
```Swift
class Solution {
    func longestPalindromeSubseq(_ s: String) -> Int {
        var memo : [[Int?]] = Array(repeating: Array(repeating: nil, count: s.count), count: s.count)
        return longest(Array(s), 0, s.count-1, &memo)
    }
    
    func longest(_ s: [Character], _ start: Int, _ end: Int, _ memo: inout [[Int?]]) -> Int {
        switch start {
            case end+1...s.count:
            return 0
            case end:
            return 1
            default:
            break
        }
        if let result = memo[start][end] {
            return result
        }
        if s[start] == s[end] {
            memo[start][end] = longest(s, start+1, end-1, &memo) + 2
        } else {
            memo[start][end] = max(longest(s, start+1, end, &memo), longest(s, start, end-1, &memo))
        }
        return memo[start][end]!
    }
}
```
__Iterative O(s^2) Time, O(s^2) Space:__
```Swift
class Solution {
    func longestPalindromeSubseq(_ s: String) -> Int {
        var memo = Array(repeating: Array(repeating: 0, count: s.count), count: s.count)
        let s = Array(s)
        for length in stride(from: 1, through: s.count, by: 1) {
            for start in stride(from: 0, through: s.count, by: 1) where start+length <= s.count {
                let end = start + length - 1
                if start == end {
                    memo[start][end] = 1
                    continue
                }
                if s[start] == s[end] {
                    memo[start][end] = memo[start+1][end-1] + 2
                } else {
                    memo[start][end] = max(memo[start+1][end], memo[start][end-1])
                }
            }
        }
        return memo.first!.last!
    }
}
```