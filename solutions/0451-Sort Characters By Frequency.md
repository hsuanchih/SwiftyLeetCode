
### Sort Characters By Frequency

Given a string, sort it in decreasing order based on the frequency of characters.

__Example 1:__
```
Input:
"tree"

Output:
"eert"

Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
```
__Example 2:__
```
Input:
"cccaaa"

Output:
"cccaaa"

Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.
```
__Example 3:__
```
Input:
"Aabb"

Output:
"bbAa"

Explanation:
"bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.
```

### Solution
__O(s+s*log\[base 2\](s)+s) Time, O(s) Space:__
```Swift
class Solution {
    func frequencySort(_ s: String) -> String {

        // Keep the frequency of each character in s, sorted by frequency
        let lookup = s.reduce(into: [Character: Int]()) {
            $0[$1, default: 0]+=1
        }.sorted { $0.value > $1.value }

        // Construct result based on the frequency of each character
        var i = 0, result = Array(s)
        for (key, value) in lookup {
            for _ in 0..<value {
                result[i] = key
                i+=1
            }
        }
        return String(result)
    }
}
```