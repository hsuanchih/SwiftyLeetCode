
### Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

__Example 1:__
```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```
__Example 2:__
```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

__Constraints:__
* `1 <= strs.length <= 200`
* `0 <= strs[i].length <= 200`
* `strs[i]` consists of only lowercase English letters.

### Solution
__O(strs) Time:__
```Swift
class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        switch strs.count {
            case 0: return ""
            case 1: return strs.first!
            default: break
        }
        let strs = strs.map(Array.init)
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
__O(strs) Time:__
```Swift
class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        let strs: [[Character]] = strs.map(Array.init)
        var index: Int = 0
        var result: [Character] = []
        while true {
            var char: Character?
            for str in strs {
                if index >= str.count || (char != nil && char != str[index]) {
                    return String(result)
                } else {
                    char = str[index]
                }
            }
            if let char {
                result.append(char)
                index += 1
            } else {
                fatalError()
            }
        }
        fatalError()
    }
}
```