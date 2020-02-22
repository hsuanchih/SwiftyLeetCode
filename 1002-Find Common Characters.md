
### Find Common Characters

Given an array `A` of strings made only from lowercase letters, return a list of all characters that show up in all strings within the list __(including duplicates)__.</br> 
For example, if a character occurs 3 times in all strings but not 4 times, you need to include that character three times in the final answer.

You may return the answer in any order.

__Example 1:__
```
Input: ["bella","label","roller"]
Output: ["e","l","l"]
```
__Example 2:__
```
Input: ["cool","lock","cook"]
Output: ["c","o"]
```

__Note:__
1. `1 <= A.length <= 100`
2. `1 <= A[i].length <= 100`
3. `A[i][j]` is a lowercase letter

### Solution
__O(n*(k+26)):__
```Swift
class Solution {
    func commonChars(_ A: [String]) -> [String] {

        // minimum keeps track of the global minimum occurences of each character in each string
        var minimum : [Int] = Array(repeating: Int.max, count: 26) 

        // Iterate through each string
        A.forEach {

            // For each string, tally the number of occurences of each character
            var tally : [Int] = Array(repeating: 0, count: 26)
            for char in $0 {
                tally[char.offset]+=1
            }

            // Update the global minimum of occurences of each character
            for offset in 0..<26 {
                minimum[offset] = min(minimum[offset], tally[offset])
            }
        }

        // Use the globla minimum to construct the result
        var result : [String] = []
        for (index, value) in minimum.enumerated() {
            for _ in 0..<value {
                result.append(String(Character(offset: index)))
            }
        }
        return result
    }
}
```