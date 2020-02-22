
### Word Break

Given a __non-empty__ string *s* and a dictionary *wordDict* containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

__Note:__
* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.

__Example 1:__
```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```
__Example 2:__
```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```
__Example 3:__
```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

### Solution
__Exponential:__
```Swift
class Solution {
    func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {
        return solve(Array(s), 0, Set(wordDict.map(ArraySlice.init)))
    }
    
    func solve(_ s: [Character], _ index: Int, _ wordDict: Set<ArraySlice<Character>>) -> Bool {
        switch index {
            case s.count:
            return true
            case s.count+1...Int.max:
            return false
            default:
            break
        }
        for i in index...s.count {
            if wordDict.contains(s[index..<i]) {
                if solve(s, i, wordDict) {
                    return true
                }
            }
        }
        return false
    }
}
```
__Polynomial Bottom-Up Recursive:__
```Swift
class Solution {
    func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {
        var memo : [Bool?] = Array(repeating: nil, count: s.count)
        return solve(Array(s), 0, Set(wordDict.map(ArraySlice.init)), &memo)
    }
    
    func solve(_ s: [Character], _ index: Int, _ wordDict: Set<ArraySlice<Character>>, _ memo: inout [Bool?]) -> Bool {
        switch index {
            case s.count:
            return true
            case s.count+1...Int.max:
            return false
            default:
            break
        }
        if let result = memo[index] {
            return result
        }
        for i in index...s.count {
            if wordDict.contains(s[index..<i]) {
                if solve(s, i, wordDict, &memo) {
                    memo[index] = true
                    return memo[index]!
                }
            }
        }
        memo[index] = false
        return memo[index]!
    }
}
```
__Polynomial Bottom-Up Iterative:__
```Swift
class Solution {
    func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {
        let s = Array(s), wordDict : Set<ArraySlice<Character>> = Set(wordDict.map(ArraySlice.init))
        var memo : [Bool] = Array(repeating: false, count: s.count)
        for i in 0..<s.count {
            for word in wordDict where i+word.count <= s.count {
                if s[i..<i+word.count] == word {
                    if i == 0 || memo[i-1] {
                        memo[i+word.count-1] = true
                    }
                }
            }
        }
        return memo.last!
    }
}
```