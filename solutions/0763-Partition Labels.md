
### Partition Labels

A string `S` of lowercase letters is given. We want to partition this string into</br> 
as many parts as possible so that each letter appears in at most one part, and return</br> 
a list of integers representing the size of these parts.

__Example 1:__
```
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
```

__Note:__
1. `S` will have length in range `[1, 500]`.
2. `S` will consist of lowercase letters (`'a'` to `'z'`) only.

### Solution
__O(S^2) Time, O(1) Space:__
```Swift
class Solution {
    func partitionLabels(_ S: String) -> [Int] {
        let S = Array(S)

        // left represents the left boundary of the current partition
        // right extends the right boundary of the current partition
        var left = 0, right = 0, result : [Int] = []

        // For each starting point, find the right-most index of the same character
        for start in 0..<S.count {
            for end in start..<S.count {

                // Same character is found, extend the right boundary
                if S[end] == S[start] {
                    right = max(end, right)
                }
            }

            // If start index reaches the right boundary, we've found one partition
            // with characters only contained within the same partition
            // Update result with length of the partition, and reset the left boundary
            if start == right {
                result.append(right-left+1)
                left = right+1
            }
        }
        return result
    }
}
```
__O(S) Time, O(S) Space:__
```Swift
extension Character {
    var offset : Int {
        return Int(asciiValue!-Character("a").asciiValue!)
    }
}

class Solution {
    func partitionLabels(_ S: String) -> [Int] {

        // lookup stores the right-most index of each character in S
        var lookup : [Int] = Array(repeating: 0, count: 26)
        for (index, char) in S.enumerated() {
            lookup[char.offset] = index
        }

        // start represents the left boundary of the current partition
        // end extends the right boundary of the current partition
        var start = 0, end = 0, result : [Int] = []

        // Iterate through each character of S
        for (index, char) in S.enumerated() {

            // Extend the right boundary of the current partition
            end = max(end, lookup[char.offset])

            // If index reaches the right boundary, we've found one partition
            // with characters only contained within the same partition
            // Update result with length of the partition, and reset the left boundary
            if index == end {
                result.append(end-start+1)
                start = end+1
            }
        }
        return result
    }
}
```