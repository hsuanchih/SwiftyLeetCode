
### Interleaving String

Given *s1*, *s2*, *s3*, find whether *s3* is formed by the interleaving of *s1* and *s2*.

__Example 1:__
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
```
__Example 2:__
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
```

### Solution
__O(2^(s3)) Time, O(1) Space - Bottom-Up Recursive:__
```Swift
class Solution {
    func isInterleave(_ s1: String, _ s2: String, _ s3: String) -> Bool {
        let s1 = Array(s1), s2 = Array(s2), s3 = Array(s3)
        if s1.count + s2.count != s3.count {
            return false
        }
        return canInterleave(s1, s2, s3, 0, 0, 0)
    }
    
    func canInterleave(_ s1: [Character], _ s2: [Character], _ s3: [Character], _ i: Int, _ j: Int, _ k: Int) -> Bool {
        switch (i, j) {
            case (s1.count, s2.count):
            return true
            case (s1.count, _) where s2[j] == s3[k]:
            return canInterleave(s1, s2, s3, i, j+1, k+1)
            case (_, s2.count) where s1[i] == s3[k]:
            return canInterleave(s1, s2, s3, i+1, j, k+1)
            case (0..<s1.count, 0..<s2.count):
            if s1[i] == s3[k] && canInterleave(s1, s2, s3, i+1, j, k+1) {
                return true
            }
            if s2[j] == s3[k] && canInterleave(s1, s2, s3, i, j+1, k+1) {
                return true
            }
            fallthrough
            default:
            return false
        }
    }
}
```
__O((s3)^2) Time, O((s3)^2) Space - Bottom-Up Recursive:__
```Swift
class Solution {
    func isInterleave(_ s1: String, _ s2: String, _ s3: String) -> Bool {
        let s1 = Array(s1), s2 = Array(s2), s3 = Array(s3)
        var memo : [[Bool?]] = Array(repeating: Array(repeating: nil, count: s2.count+1), count: s1.count+1)
        if s1.count + s2.count != s3.count {
            return false
        }
        return canInterleave(s1, s2, s3, 0, 0, 0, &memo)
    }
    
    func canInterleave(_ s1: [Character], _ s2: [Character], _ s3: [Character], _ i: Int, _ j: Int, _ k: Int, _ memo: inout [[Bool?]]) -> Bool {
        
        if let result = memo[i][j] {
            return result
        }
        switch (i, j) {
            case (s1.count, s2.count):
            memo[i][j] = true
            case (s1.count, _) where s2[j] == s3[k]:
            memo[i][j] = canInterleave(s1, s2, s3, i, j+1, k+1, &memo)
            case (_, s2.count) where s1[i] == s3[k]:
            memo[i][j] = canInterleave(s1, s2, s3, i+1, j, k+1, &memo)
            case (0..<s1.count, 0..<s2.count):
            if s1[i] == s3[k] && canInterleave(s1, s2, s3, i+1, j, k+1, &memo) {
                memo[i][j] = true
                break
            }
            if s2[j] == s3[k] && canInterleave(s1, s2, s3, i, j+1, k+1, &memo) {
                memo[i][j] = true
                break
            }
            fallthrough
            default:
            memo[i][j] = false
        }
        return memo[i][j]!
    }
}
```