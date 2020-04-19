
### Number of Matching Subsequences

Given string `S` and a dictionary of words `words`, find the number of `words[i]` that is a subsequence of `S`.

__Example:__
```
Input: 
S = "abcde"
words = ["a", "bb", "acd", "ace"]
Output: 3
Explanation: There are three words in words that are a subsequence of S: "a", "acd", "ace".
```

__Note:__
* All words in `words` and `S` will only consists of lowercase letters.
* The length of `S` will be in the range of `[1, 50000]`.
* The length of `words` will be in the range of `[1, 5000]`.
* The length of `words[i]` will be in the range of `[1, 50]`.

### Solution
__O(2^S) Time, O(words) Space - Brute-Force:__
```Swift
class Solution {
    func numMatchingSubseq(_ S: String, _ words: [String]) -> Int {
        let count = words.count
        var words = Set(words), temp = ""
        solve(Array(S), &words, &temp, 0)
        return count-words.count
    }
    
    func solve(_ S: [Character], _ words: inout Set<String>, _ temp: inout String, _ index: Int) {
        if words.contains(temp) {
            words.remove(temp)
        }
        if index == S.count {
            return
        }
        solve(S, &words, &temp, index+1)
        temp.append(S[index])
        solve(S, &words, &temp, index+1)
        temp.removeLast()
    }
}
```
__O((S+c)*words) Time, O(c) Space - Improved Brute-Force:__
```Swift
class Solution {
    func numMatchingSubseq(_ S: String, _ words: [String]) -> Int {
        var result = 0
        words.forEach {
            let word = Array($0)
            var i = 0
            for char in S {
                if char == word[i] {
                    i+=1
                }
                if i == word.count {
                    result+=1
                    break
                }
            }
        }
        return result
    }
}
```
__O((log\[base 2\](S)\*c)*words+S) Time, O(S) Space - Binary-Search:__
```Swift
extension Array {
    func binarySearch(_ start: Int, _ end: Int, _ transform: (Int, inout Int, inout Int)->Void) -> (start: Int, end: Int) {
        guard start >= 0 && end >= 0 else { return (-1, -1) }
        var start = start, end = end
        while start+1 < end {
            transform(start+(end-start)/2, &start, &end)
        }
        return (start, end)
    }
}

class Solution {
    func numMatchingSubseq(_ S: String, _ words: [String]) -> Int {

        // Store the indices of each Character in the look-up
        var lookup : [Character: [Int]] = [:]
        for (index, value) in S.enumerated() {
            lookup[value, default: [Int]()].append(index)
        }

        // Iterate through all words
        var result = 0
        words.forEach {

            // curr stores the current index in S
            // curr starts at -1
            var curr = -1

            // For each character in word, use binary search to find the index of the of
            // the same character in S that is greater than curr
            for (index, value) in $0.enumerated() {
                guard let indices = lookup[value] else { break }
                switch (indices.binarySearch(0, indices.count-1) { (mid, start, end) in
                    if indices[mid] > curr {
                        end = mid
                    } else {
                        start = mid
                    }
                }) {
                case let (start, end):
                    // Update curr if such next index is found, otherwise set curr to -1
                    // to indicate that no such index is found
                    switch (indices[start], indices[end]) {
                        case (curr+1...Int.max, _):
                        curr = indices[start]
                        case (_, curr+1...Int.max):
                        curr = indices[end]
                        default:
                        curr = -1
                    }
                }

                // Break out of the iteration if next index is not found
                if curr == -1 {
                    break
                }

                // If we've reached the end of the word, there is a subsequence match
                // Update result
                if index == $0.count-1 {
                    result+=1
                }
            }
        }
        return result
    }
}
```