
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

__Constraints:__
* `1 <= s.length <= 300`
* `1 <= wordDict.length <= 1000`
* `1 <= wordDict[i].length <= 20`
* `s` and `wordDict[i]` consist of only lowercase English letters.
* All the strings of `wordDict` are __unique__.

### Solution
__O(2^(s-2)) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {

        // Convert s into [Character] for ease of indexing, 
        // and wordDict into ArraySlice<Character> for ease of comparison
        return wordBreak(Array(s), 0, Set(wordDict.map(ArraySlice.init)))
    }
    
    func wordBreak(_ s: [Character], _ index: Int, _ wordDict: Set<ArraySlice<Character>>) -> Bool {

        for i in index..<s.count {

            // If s[index...i] matches a word in wordDict, continue searching on
            // in s starting with index i+1
            // If any one search ends up with match, we've found a solution:
            // early return true
            if wordDict.contains(s[index...i]) && wordBreak(s, i+1, wordDict) {
                return true
            }
        }

        // Return true only if index reaches s.count, otherwise we've not found
        // a match starting at index
        return index == s.count
    }
}
```
__O(s^2) Time, O(s) Space - Bottom-Up Recursive, Top-Down Memoization:__
```Swift
class Solution {
    func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {
        var memo : [Bool?] = Array(repeating: nil, count: s.count)
        return wordBreak(Array(s), 0, Set(wordDict.map(ArraySlice.init)), &memo)
    }
    
    func wordBreak(_ s: [Character], _ index: Int, _ wordDict: Set<ArraySlice<Character>>, _ memo: inout [Bool?]) -> Bool {
        if index == s.count {
            return true
        }
        if let result = memo[index] {
            return result
        }
        memo[index] = false
        for i in index..<s.count {
            if wordDict.contains(s[index...i]) && wordBreak(s, i+1, wordDict, &memo) {
                memo[index] = true
                break
            }
        }
        return memo[index]!
    }
}
```
__O(s^2) Time, O(s) Space - Bottom-Up Iterative, Bottom-Up Memoization:__
```Swift
class Solution {
    func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {
        let s = Array(s), wordDict : Set<ArraySlice<Character>> = Set(wordDict.map(ArraySlice.init))
        var memo : [Bool] = Array(repeating: false, count: s.count)
        
        // Iterate through s, and check if there is any word in wordDict 
        // that matches a substring in s starting at index i:
        // If there is, then the substring from 0..<i+word.count-1 is a match
        // only if 0..<i-1 is also a match - update memo array
        for i in 0..<s.count {
            for word in wordDict where i+word.count <= s.count {
                if s[i..<i+word.count] == word {
                    if i == 0 || memo[i-1] {
                        memo[i+word.count-1] = true
                    }
                }
            }
        }

        // We have a solution only if 0..<s.count-1 is a match
        return memo.last!
    }
}
```