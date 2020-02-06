
### Group Anagrams

Given an array of strings, group anagrams together.

__Example:__
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```
__Note:__
* All inputs will be in lowercase.
* The order of your output does not matter.

### Solution
__O(clogc*n):__
```Swift
class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        var result : [[Character]:[String]] = [:]
        for str in strs {
            result[str.sorted(), default: []].append(str)
        }
        return result.map { $1 }
    }
}
```
__O(c*n):__
```Swift
class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        var result : [[Int]: [String]] = [:]
        for str in strs {
            var count = Array(repeating: 0, count: 26)
            for char in str {
                let index = Int(char.asciiValue! - Character("a").asciiValue!)
                count[index] += 1
            }
            result[count, default: []].append(str)
        }
        return result.map { $1 }
    }
}
```