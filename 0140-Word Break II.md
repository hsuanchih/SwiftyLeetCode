
### Word Break II

Given a __non-empty__ string s and a dictionary *wordDict* containing a list of __non-empty__ words,</br> 
add spaces in s to construct a sentence where each word is a valid dictionary word.</br> 
Return all such possible sentences.

__Note:__
* The same word in the dictionary may be reused multiple times in the segmentation.
* You may assume the dictionary does not contain duplicate words.

__Example 1:__
```
Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]
```
__Example 2:__
```
Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.
```
__Example 3:__
```
Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

### Solution
__Exponential:__
```Swift
class Solution {
    func wordBreak(_ s: String, _ wordDict: [String]) -> [String] {
        let wordDict = Set(wordDict.map(ArraySlice.init))
        var temp = [ArraySlice<Character>](), result = [String]()
        wordBreak(Array(s), 0, wordDict, &temp, &result)
        return result
    }
    
    func wordBreak(_ s: [Character], _ index: Int, _ wordDict: Set<ArraySlice<Character>>, _ temp: inout [ArraySlice<Character>], _ result: inout [String]) {
        if index == s.count {
            let word = temp.reduce("") {
                $0.isEmpty ? String($1) : "\($0) \(String($1))"
            }
            result.append(word)
            return
        }
        for i in index..<s.count {
            let word = s[index...i]
            if wordDict.contains(word) {
                temp.append(word)
                wordBreak(s, i+1, wordDict, &temp, &result)
                temp.removeLast()
            }
        }
    }
}
```
__Polynomial Bottom-Up Recursive:__
```Swift
class Solution {
    func wordBreak(_ s: String, _ wordDict: [String]) -> [String] {
        var memo : [[String]?] = Array(repeating: nil, count: s.count)
        let wordDict = Set(wordDict.map(ArraySlice.init))
        return wordBreak(Array(s), 0, wordDict, &memo)
    }
    
    func wordBreak(_ s: [Character], _ index: Int, _ wordDict: Set<ArraySlice<Character>>, _ memo: inout [[String]?]) -> [String] {
        if let result = memo[index] {
            return result
        }
        var result : [String] = []
        for i in index..<s.count {
            let word = s[index...i]
            if wordDict.contains(word) {
                if i == s.count-1 {
                    result.append(String(word))
                } else {
                    wordBreak(s, i+1, wordDict, &memo).forEach {
                        result.append("\(String(word)) \($0)")
                    }
                }
            }
        }
        memo[index] = result
        return result
    }
}
```