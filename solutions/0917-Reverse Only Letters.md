
### Reverse Only Letters

Given a string `S`, return the "reversed" string where all characters that are not a letter stay in the same place, and all letters reverse their positions.

__Example 1:__
```
Input: "ab-cd"
Output: "dc-ba"
```
__Example 2:__
```
Input: "a-bC-dEf-ghIj"
Output: "j-Ih-gfE-dCba"
```
__Example 3:__
```
Input: "Test1ng-Leet=code-Q!"
Output: "Qedo1ct-eeLg=ntse-T!"
```

__Note:__
1. `S.length <= 100`
2. `33 <= S[i].ASCIIcode <= 122`
3. `S` doesn't contain `\` or `"`

### Solution
__O(S) Time:__
```Swift
class Solution {
    func reverseOnlyLetters(_ S: String) -> String {
        var S = Array(S), start = 0, end = S.count-1
        while start < end {
            while start < end && !S[start].isLetter {
                start+=1
            }
            while start < end && !S[end].isLetter {
                end-=1
            }
            S.swapAt(start, end)
            start+=1
            end-=1
        }
        return String(S)
    }
}
```