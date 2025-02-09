
### Word Ladder

A __transformation sequence__ from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:
* Every adjacent pair of words differs by a single letter.
* Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
* `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return the number of words in the __shortest transformation sequence__ from `beginWord` to `endWord`, or `0` if no such sequence exists.

__Example 1:__
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.
```
__Example 2:__
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
```

__Constraints:__
* `1 <= beginWord.length <= 10`
* `endWord.length == beginWord.length`
* `1 <= wordList.length <= 5000`
* `wordList[i].length == beginWord.length`
* `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
* `beginWord != endWord`
* All the words in `wordList` are __unique__.

### Solution
__O(n\*k\*26):__
```Swift

// Character extension to convert ascii value to Character
extension Character {
    static func from(_ asciiValue: UInt8) -> Character {
        return Character(UnicodeScalar(asciiValue))
    }
}

class Solution {
    func ladderLength(_ beginWord: String, _ endWord: String, _ wordList: [String]) -> Int {
        let beginWord = Array(beginWord), endWord = Array(endWord)
        var wordList = Set(wordList.map(Array.init)), queue = [beginWord], result = 0
        while !queue.isEmpty {
            result+=1
            let size = queue.count
            for _ in stride(from: 0, to: size, by: 1) {
                let word = queue.removeFirst()

                // End word matches the current word, return result
                if word == endWord {
                    return result
                }

                // Replace each character of word with a-z to form the next transform,
                // if the transform exists in wordList, add it to the queue for processing
                // in the next frontier, and discard the transform from wordList (aka, mark as visited)
                // to prevent graph cycle
                var curr = word
                for index in stride(from: 0, to: curr.count, by: 1) {
                    let original = curr[index], a = Character("a").asciiValue!
                    for offset in UInt8(0)..<26 {
                        curr[index] = Character.from(a+offset)
                        if wordList.contains(curr) {
                            queue.append(curr)
                            wordList.remove(curr)
                        }
                    }
                    curr[index] = original
                }
            }
        }
        return 0
    }
}
```
__O(n*k):__
```Swift
extension Array where Element == Character {
    var transforms : Set<[Character]> {
        var array = self, transforms : Set<[Character]> = []
        for index in 0..<count {
            let original = array[index]
            array[index] = "*"
            transforms.insert(array)
            array[index] = original
        }
        return transforms
    }
}

class Solution {
    func ladderLength(_ beginWord: String, _ endWord: String, _ wordList: [String]) -> Int {
        let beginWord = Array(beginWord), endWord = Array(endWord)
        var wordList = wordList
            .lazy
            .map(Array.init)
            .reduce(into: [[Character] : Set<[Character]>]()) { (result, word) in
                word.transforms.forEach {
                    result[$0, default: Set<[Character]>()].insert(word)
                }
            },
        queue = [beginWord],
        result = 0
        
        while !queue.isEmpty {
            result+=1
            let size = queue.count
            for _ in stride(from: 0, to: size, by: 1) {
                let curr = queue.removeFirst()
                if curr == endWord {
                    return result
                }
                curr.transforms.forEach {
                    if let nextWords = wordList[$0] {
                        queue.append(contentsOf: Array(nextWords))
                        wordList.removeValue(forKey: $0)
                    }
                }
            }
        }
        return 0
    }
}
```