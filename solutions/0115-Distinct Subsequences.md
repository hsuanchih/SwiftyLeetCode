
### Distinct Subsequences

Given a string __S__ and a string __T__, count the number of distinct subsequences of __S__ which equals __T__.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ACE"` is a subsequence of `"ABCDE"` while `"AEC"` is not).

__Example 1:__
```
Input: S = "rabbbit", T = "rabbit"
Output: 3
Explanation:

As shown below, there are 3 ways you can generate "rabbit" from S.
(The caret symbol ^ means the chosen letters)

rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^
```
__Example 2:__
```
Input: S = "babgbag", T = "bag"
Output: 5
Explanation:

As shown below, there are 5 ways you can generate "bag" from S.
(The caret symbol ^ means the chosen letters)

babgbag
^^ ^
babgbag
^^    ^
babgbag
^    ^^
babgbag
  ^  ^^
babgbag
    ^^^
```

### Solution
__O(2^t) Time, O(1) Space Bottom-Up Recursive:__
```Swift
class Solution {
    func numDistinct(_ s: String, _ t: String) -> Int {
        let sLength = s.count, tLength = t.count
        if sLength < tLength {
            return 0
        }
        return numDistinct(Array(s), Array(t), 0, 0)
    }
    
    func numDistinct(_ s: [Character], _ t: [Character], _ sIndex: Int, _ tIndex: Int) -> Int {
        if tIndex == t.count {
            return 1
        }
        if sIndex == s.count {
            return 0
        }
        var result = 0
        for index in sIndex..<s.count {
            if t[tIndex] == s[index] {
                result += numDistinct(s, t, index+1, tIndex+1)
            }
        }
        return result
    }
}
```
__O(s\*t) Time, O(s*t) Space Bottom-Up Recursive:__
```Swift
class Solution {
    func numDistinct(_ s: String, _ t: String) -> Int {
        var memo : [[Int?]] = Array(repeating: Array(repeating: nil, count: t.count), count: s.count)
        return numDistinct(Array(s), Array(t), 0, 0, &memo)
    }
    
    func numDistinct(_ s: [Character], _ t: [Character], _ sIndex: Int, _ tIndex: Int, _ memo: inout [[Int?]]) -> Int {
        if tIndex == t.count {
            return 1
        }
        if sIndex == s.count {
            return 0
        }
        if let result = memo[sIndex][tIndex] {
            return result
        }
        var count = 0
        for index in sIndex..<s.count {
            if t[tIndex] == s[index] {
                count += numDistinct(s, t, index+1, tIndex+1, &memo)
            }
        }
        memo[sIndex][tIndex] = count
        return memo[sIndex][tIndex]!
    }
}
```