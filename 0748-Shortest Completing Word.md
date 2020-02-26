
### Shortest Completing Word

Find the minimum length word from a given dictionary `words`, which has all the letters from the string `licensePlate`.</br> 
Such a word is said to complete the given string `licensePlate`

Here, for letters we ignore case. For example, `"P"` on the `licensePlate` still matches `"p" `on the word.

It is guaranteed an answer exists. If there are multiple answers, return the one that occurs first in the array.

The license plate might have the same letter occurring multiple times.</br> 
For example, given a `licensePlate` of `"PP"`, the word `"pair"` does not complete the `licensePlate`, but the word `"supper"` does.

__Example 1:__
```
Input: licensePlate = "1s3 PSt", words = ["step", "steps", "stripe", "stepple"]
Output: "steps"
Explanation: The smallest length word that contains the letters "S", "P", "S", and "T".
Note that the answer is not "step", because the letter "s" must occur in the word twice.
Also note that we ignored case for the purposes of comparing whether a letter exists in the word.
```
__Example 2:__
```
Input: licensePlate = "1s3 456", words = ["looks", "pest", "stew", "show"]
Output: "pest"
Explanation: There are 3 smallest length words that contains the letters "s".
We return the one that occurred first.
```

__Note:__
1. `licensePlate` will be a string with length in range `[1, 7]`.
2. `licensePlate` will contain digits, spaces, or letters (uppercase or lowercase).
3. `words` will have a length in the range `[10, 1000]`.
4. Every `words[i]` will consist of lowercase letters, and have length in range `[1, 15]`.

### Solution
__O(k*words+licensePlate):__
```Swift
class Solution {
    func shortestCompletingWord(_ licensePlate: String, _ words: [String]) -> String {

        // Transform characters in license plate into lookup
        let map = licensePlate.reduce(into: [Character: Int]()) {
            if $1.isLetter {
                $0[$1.lowercased().first!, default: 0]+=1
            }
        }

        var lookup = map, result = ""

        // Iterate through all words
        for word in words {

            // Iterate through characters of each word
            for char in word {

                // If character is found in the lookup, subtract corresponding character count
                // in lookup by 1
                // If count is 0, remove character from lookup
                if let count = lookup[char] {
                    if count > 1 {
                        lookup[char] = count-1
                    } else {
                        lookup.removeValue(forKey: char)
                    }
                }

                // If lookup is empty, word contains all characters in license plate
                // Update result with word if it's shorter than the previous found word
                if lookup.isEmpty {
                    if result.isEmpty || word.count < result.count {
                        result = word
                    }
                    break
                }
            }

            // Reset lookup
            lookup = map
        }
        return result
    }
}
```