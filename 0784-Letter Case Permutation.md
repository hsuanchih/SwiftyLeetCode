
### Letter Case Permutation

Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.

__Examples:__
```
Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

Input: S = "3z4"
Output: ["3z4", "3Z4"]

Input: S = "12345"
Output: ["12345"]
```
__Note:__

`S` will be a string with length between `1` and `12`.
`S` will consist only of letters or digits.

### Solution
__O(2^n):__
```Swift
class Solution {
    func letterCasePermutation(_ S: String) -> [String] {
        var S = Array(S), result : [String] = []
        solve(&S, 0, &result)
        return result
    }
    
    func solve(_ S: inout [Character], _ index: Int, _ result: inout [String]) {
        if index == S.count {
            result.append(String(S))
            return
        }
        solve(&S, index+1, &result)
        if S[index].isLetter {
            if S[index].isLowercase {
                S[index] = S[index].uppercased().first!
            } else {
                S[index] = S[index].lowercased().first!
            }
            solve(&S, index+1, &result)
        }
    }
}
```