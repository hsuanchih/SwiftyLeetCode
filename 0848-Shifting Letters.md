
### Shifting Letters

We have a string `S` of lowercase letters, and an integer array `shifts`.

Call the *shift* of a letter, the next letter in the alphabet, (wrapping around so that `'z'` becomes `'a'`). 

For example, `shift('a') = 'b'`, `shift('t') = 'u'`, and `shift('z') = 'a'`.

Now for each `shifts[i] = x`, we want to shift the first `i+1` letters of `S`, `x` times.

Return the final string after all such shifts to S are applied.

__Example 1:__
```
Input: S = "abc", shifts = [3,5,9]
Output: "rpl"
Explanation: 
We start with "abc".
After shifting the first 1 letters of S by 3, we have "dbc".
After shifting the first 2 letters of S by 5, we have "igc".
After shifting the first 3 letters of S by 9, we have "rpl", the answer.
```

__Note:__
1. `1 <= S.length = shifts.length <= 20000`
2. `0 <= shifts[i] <= 10 ^ 9`

### Solution
__O(S):__
```Swift
extension Character {
    func shifted(by shift: Int) -> Character {
        let a = Int(Character("a").asciiValue!)
        let initial = Int(asciiValue!) - a, result = (initial + shift)%26
        return Character(UnicodeScalar(result + a)!)
    }
}
class Solution {
    func shiftingLetters(_ S: String, _ shifts: [Int]) -> String {
        var S = Array(S), shift = 0
        for i in stride(from: shifts.count-1, through: 0, by: -1) {
            shift += shifts[i]
            S[i] = S[i].shifted(by: shift)
        }
        return String(S)
    }
}
```