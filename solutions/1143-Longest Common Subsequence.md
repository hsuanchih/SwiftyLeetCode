
### Longest Common Subsequence

Given two strings `text1` and `text2`, return the length of their longest common subsequence.

A *subsequence* of a string is a new string generated from the original string with some characters(can be none) deleted without changing the relative order of the remaining characters. (eg, "ace" is a subsequence of "abcde" while "aec" is not). A *common subsequence* of two strings is a subsequence that is common to both strings.

If there is no common subsequence, return 0.

__Example 1:__
```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
Example 2:

Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
Example 3:

Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

__Constraints:__
* `1 <= text1.length <= 1000`
* `1 <= text2.length <= 1000`
* The input strings consist of lowercase English characters only

### Solution
__O(2^(min(text1, text2))) Time, O(1) Space - Bottom-Up Recursive:__
```Swift
class Solution {
    func longestCommonSubsequence(_ text1: String, _ text2: String) -> Int {
        return longestCommon(Array(text1), Array(text2), 0, 0)
    }
    
    func longestCommon(_ text1: [Character], _ text2: [Character], _ index1: Int, _ index2: Int) -> Int {
        switch (index1, index2) {

        	// Base case: An empty string will have no common subsequence with another string
            case (text1.count, _), (_, text2.count):
            return 0

            // If the characters of both strings match, then we have 1 more common character amongst the strings
            // Otherwise we can advance either the index of text1 or index of text2 to find the longest common subsequence among
            // the 2 strings
            default:
            if text1[index1] == text2[index2] {
                return 1+longestCommon(text1, text2, index1+1, index2+1)
            } else {
                return max(longestCommon(text1, text2, index1+1, index2), longestCommon(text1, text2, index1, index2+1))
            }
        }
    }
}
```
__O(text1\*text2) Time, O(text1\*text2) Space - Bottom-Up Recursive, Top-Down Memoization:__
```Swift
class Solution {
    func longestCommonSubsequence(_ text1: String, _ text2: String) -> Int {
        var memo : [[Int?]] = Array(repeating: Array(repeating: nil, count: text2.count), count: text1.count)
        return longestCommon(Array(text1), Array(text2), 0, 0, &memo)
    }
    
    func longestCommon(_ text1: [Character], _ text2: [Character], _ index1: Int, _ index2: Int, _ memo: inout [[Int?]]) -> Int {
        switch (index1, index2) {
            case (text1.count, _), (_, text2.count):
            return 0
            default:
            if let result = memo[index1][index2] {
                return result
            }
            if text1[index1] == text2[index2] {
                memo[index1][index2] = 1+longestCommon(text1, text2, index1+1, index2+1, &memo)
            } else {
                memo[index1][index2] = max(longestCommon(text1, text2, index1+1, index2, &memo), longestCommon(text1, text2, index1, index2+1, &memo))
            }
            return memo[index1][index2]!
        }
    }
}
```