
### Maximum Product of Word Lengths

Given a string array `words`, find the maximum value of `length(word[i]) * length(word[j])`</br> 
where the two words do not share common letters. You may assume that each word will contain only lower case letters.</br> 
If no such two words exist, return 0.

__Example 1:__
```
Input: ["abcw","baz","foo","bar","xtfn","abcdef"]
Output: 16 
Explanation: The two words can be "abcw", "xtfn".
```
__Example 2:__
```
Input: ["a","ab","abc","d","cd","bcd","abcd"]
Output: 4 
Explanation: The two words can be "ab", "cd".
```
__Example 3:__
```
Input: ["a","aa","aaa","aaaa"]
Output: 0 
Explanation: No such pair of words.
```

### Solution
__O(words*k+words^2*(unique(k)^2), where k is the number of characters in a word:__
```Swift
class Solution {
    func maxProduct(_ words: [String]) -> Int {
        let words = words.map { (Set(Array($0)), $0.count) }
        var result = 0
        for curr in 0..<words.count {
            let currWord = words[curr]
            for i in curr+1..<words.count {
                let comparedWord = words[i]
                if currWord.0.intersection(comparedWord.0).isEmpty {
                    result = max(result, currWord.1*comparedWord.1)
                }
            }
        }
        return result
    }
}
```
__O(words*k+words^2):__
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue!-Character("a").asciiValue!)
    }
}

class Solution {
    func maxProduct(_ words: [String]) -> Int {
        
        // Convert each word into a bitmap of each character's offset 
        let words = words.map { (bitmap($0), $0.count) }
        var result = 0
        for curr in 0..<words.count {
            let currWord = words[curr]
            for i in curr+1..<words.count {
                let comparedWord = words[i]
                
                // If the bitmaps do not intersect, then the 2 words
                // do not share a common character
                if currWord.0 & comparedWord.0 == 0 {
                    result = max(result, currWord.1*comparedWord.1)
                }
            }
        }
        return result
    }
    
    // Helper method to convert each word into a bitmap
    func bitmap(_ word: String) -> Int {
        let word = Array(word)
        var result = 0
        for char in word {
            result |= 1<<char.offset
        }
        return result
    }
}
```