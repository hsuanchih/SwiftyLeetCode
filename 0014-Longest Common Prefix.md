
### Count and Say

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

__Example 1:__
```
Input: ["flower","flow","flight"]
Output: "fl"
```
__Example 2:__
```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```
__Note:__

All given inputs are in lowercase letters `a-z`.

### Solution
__O(k*n):__
```Swift
class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        switch strs.count {
            case 0: return ""
            case 1: return strs.first!
            default: break
        }
        let strs = strs.map { Array($0) }
        let minLength = strs.reduce(Int.max) { return min($0, $1.count) }
        for i in stride(from: 0, to: minLength, by: 1) {
            let curr = strs.first![i]
            for j in stride(from: 1, to: strs.count, by: 1) {
                if strs[j][i] != curr {
                    return String(strs.first![0..<i])
                }
            }
        }
        return String(strs.first![0..<minLength])
    }
}
```